---
name: event-coverage
description: Write a blog post covering a single speaker / session at a conference or event — typically from a YouTube recording — in the house style of this repo. Use when the user provides a YouTube URL (often with a `&t=NNNs` timestamp) and asks to extract a speaker's segment, write up that segment as a blog post, and/or prep accompanying images. Handles the full workflow end to end: transcript extraction, segment identification, cross-checking against the official schedule and the speaker's own publications, drafting in voice, building an image shot list, and committing to the assigned branch.
---

# Event coverage blog post

A repeatable workflow for turning a YouTube recording of a conference talk into a publishable blog post in this repo's house style. Built from the workflow that produced `2026-05-15-ai-engineer-singapore-day-1.md` and `2026-05-16-ai-engineer-singapore-vivian-balakrishnan.md` — both of which are the canonical examples to imitate.

## Inputs to confirm with the user

Before doing anything expensive, confirm:

1. **The YouTube URL** of the recording. If they gave a `&t=NNNs` timestamp, that is the user's hint of where the segment of interest begins.
2. **Which speaker / talk** they want covered. One post per speaker is the default — don't bundle multiple talks unless explicitly asked.
3. **Series framing** — is this part of a multi-day / multi-post series? If yes, look at the most recent existing post in the repo and match its title pattern (e.g. `"AI Engineer Singapore, Day N: [headline]"`).

Don't ask if you can already infer the answer (e.g. they linked a specific timestamp, they named the speaker). Reserve the question for when something is genuinely ambiguous.

## Step 1 — Pull a timestamped transcript via Apify

YouTube blocks `WebFetch` and `yt-dlp` from the remote execution sandbox. Use the **Apify MCP server** instead. The actor with the best cost/accuracy tradeoff is `starvibe/youtube-video-transcript`.

**Call it without `include_transcript_text: true`** — that gives you the timestamped `transcript[]` array, which is what you want. Setting `include_transcript_text: true` gives you a single plain-text string with no timestamps, which is useless for finding segment boundaries.

```
call-actor:
  actor: starvibe/youtube-video-transcript
  input: { "youtube_url": "<URL>", "language": "en" }
```

The actor returns a `datasetId`. Read with `get-actor-output` — but **the result will almost always exceed token limits** for a ~10 hour conference video. When it does, the tool writes the JSON to a file and gives you the path. Work with the file via `jq` rather than re-reading it. Schema of each segment: `{ start, end, duration, text }` in seconds.

## Step 2 — Locate the speaker's segment

Use the user's `&t=NNNs` hint as a starting point, then bracket a wider window:

```bash
jq -r '.items[0].transcript[] | select(.start >= 2400 and .start <= 5500) | "\(.start)\t\(.text)"' "$FILE" > /tmp/segs.tsv
```

Find segment boundaries by grepping for:
- The host's **introduction** phrase ("please welcome", "introduce", "honored to introduce", speaker's name)
- The **next speaker's** introduction (marks the end of this segment)
- Distinctive phrases the speaker uses (their company, their topic, their hook)

Auto-generated transcripts are unreliable for **proper nouns**. Expect to see things like `Yan Leon` for "Yann LeCun", `Bailey's` for "Baileys", `Neman` for "mnemon". Note suspicious-looking names for the cross-check step.

## Step 3 — Cross-check against authoritative sources

Auto-transcripts get names, project titles, and technical terms wrong. Three sources to verify against, in priority order:

1. **The event's official schedule page.** Get the canonical talk title, the time slot, the track, and the names of the surrounding speakers. Use the schedule as the source of truth for spelling of names and company names. (If `WebFetch` is blocked, the user can usually paste a screenshot of the schedule — accept that as the source.)
2. **The speaker's own publications.** If they've posted a GitHub gist, a blog post, a Facebook post, or any technical writeup about what they discussed, treat it as **the** source of truth for stack details, tool names, hardware specs, and architecture. Auto-transcripts will mishear these every time.
3. **Press coverage.** Useful for additional quotes and reactions, but lower priority — journalists paraphrase.

Build a mental diff between what the transcript says and what authoritative sources say. Fix the transcript-derived spellings in the post; don't assume the auto-transcript is right.

## Step 4 — Draft the post in house style

**Always read the most recent post in the repo first** to lock in voice, structure, length, and any series conventions. Don't write from scratch.

### House style — what to do

- **Direct quotes** for high-impact lines, drawn verbatim from the transcript. Use blockquotes (`>`) for the marquee lines and inline italics for shorter bits.
- **Observational tone.** Describe what the speaker said and what stood out; let readers draw their own conclusions about whether and how it applies to them.
- **Structure usually follows the speaker's structure.** If they opened with "three takeaways", organise the post around those three. Don't impose your own scaffolding on a talk that already has one.
- **Title format** matches the existing series. As of writing: `"AI Engineer Singapore, Day N: [headline]"` for this series. Match exactly.
- **One short orienting paragraph** near the top noting this post covers a single session inside a larger event, with a link to the [full schedule] and a brief flavour of what else ran that day. Helps non-attendees place the talk in context.
- **Closing pattern**: a "more posts coming" beat (specific session names if possible), followed by an in-person invitation that ends the post. Mirror the Day 1 closing — read it before writing yours.

### House style — what to avoid

- **No prescriptive / preachy phrasings.** Do not write "I want senior leadership to hear this", "I want everyone at [org] to take away", "you should…", "every [role] needs to…". The user has explicitly flagged these as off-tone. Reframe to observational ("the line that landed", "the argument is essentially", "this generalises to anyone who…").
- **No invented quotes.** If you don't have it in the transcript or in a published source, don't put it in quote marks. Paraphrase clearly attributed instead.
- **No invented stats.** If press coverage said `$80 hardware / $5–20 per month` and the talk did not, do not cite those numbers as if from the talk — they are a separate source. The post should be primarily built from the speaker's own words.
- **No "Three things I want SMU to take away" style sections.** If you want a recap at the end, use observational framings like "Three threads worth pulling on".
- **No comments / notes / "TODO" markers in the published post.** If something needs verification, ask the user before publishing.

### Voice calibration

Read `2026-05-16-ai-engineer-singapore-vivian-balakrishnan.md` for the single-speaker single-talk shape, and `2026-05-15-ai-engineer-singapore-day-1.md` for the full-day recap shape. Match whichever is closer to the requested post.

## Step 5 — Build the image shot list

Create a **sibling file** named `<post-filename>.images.md`. Don't try to download the video and extract frames — the sandbox blocks YouTube, and Apify video-download actors charge by file size (typically $3–7 for a full conference recording, mostly bytes you don't need).

Instead, hand the user a markdown table with these columns:

| # | Blog section | Timestamp | What's on screen | Suggested filename | Suggested caption |

For each row:
- **Timestamp** is a clickable `[MM:SS](https://www.youtube.com/watch?v=ID&t=NNNs)` link — they click and it jumps straight to the right second in YouTube.
- **What's on screen** tells them what to capture (a specific slide, a reaction shot, an architecture diagram).
- **Filename** is zero-padded so files sort correctly: `01-intro.jpg`, `02-three-takeaways.jpg`, etc.
- **Caption** is written in the post's voice and is ready to paste under the image.

Aim for **10–15 images** per post — one per major section. Include a short "tip for clean screenshots" at the top of the file (theatre mode → pause → fullscreen → mouse off-screen so the player chrome fades). Mention that **slide exports from the speaker's own deck (if published) will always be sharper than YouTube frames** — worth checking first.

## Step 6 — Write a short description

When asked for a description (for OG tags, social cards, post excerpts), offer **3–4 length variants** rather than a single answer:

- **Tiny** (~90 chars) — for tweet text or breadcrumbs
- **Punchy** (~170 chars) — for Twitter/X cards, LinkedIn previews
- **Standard** (~250 chars) — for blog excerpt / OG description (the default)
- **Long** (~400 chars) — for landing pages, LinkedIn article descriptions

All should match the post's voice. Recommend the standard one unless the user has flagged a specific use case.

## Step 7 — Commit and push

The session almost always runs on an assigned development branch (look for the branch name in the task description). Commit each meaningful change as you go rather than batching everything — separate commits for `add post`, `correct against transcript`, `correct against gist`, `update title`, `soften tone`, etc. Push to the assigned branch with `-u origin <branch>` on the first push.

**Do not open a PR unless the user explicitly asks for one.**

## Anti-patterns observed in earlier runs

- **Trusting press paraphrases over the speaker's actual words.** The first draft I wrote was built off SCMP and OfficeChai paraphrases because the YouTube transcript wasn't yet pulled. Half the quotes turned out to be wrong when checked against the real transcript. **Always get the transcript first.**
- **Trusting the auto-transcript on proper nouns.** "mnemon" → "Neman", "Gavriel" → "Gabriel", "Yann LeCun" → "Yan Leon" — every one of these went out in a draft before being caught at the cross-check step.
- **Lecturing the reader.** Early drafts had "I want senior leadership to hear", "I want everyone at SMU to take away", "the most senior-leadership-flavoured argument" — all reframed after the user flagged the tone.
- **Over-using a good joke.** A clever framing (e.g. "forward deployed minister") works twice — in the title and a single callback. Five times reads like a slogan. Land it twice.
- **Burning Apify credits on full video downloads.** Don't. Hand the user a timestamped shot list instead.

## Quick checklist before declaring done

- [ ] Title matches the series convention.
- [ ] First paragraph orients the reader; second paragraph delivers the hook from the title.
- [ ] One short context paragraph linking the official schedule.
- [ ] Every direct quote is verifiable in the transcript.
- [ ] Proper nouns cross-checked against schedule / gist / published source.
- [ ] No prescriptive "I want X to learn" phrasings.
- [ ] Closing: "more posts coming" beat, ending on the in-person invitation.
- [ ] Sibling `*.images.md` file with 10–15 timestamped rows.
- [ ] Committed and pushed to the assigned branch. No PR unless asked.
