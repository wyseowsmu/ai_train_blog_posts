# Image shot list — AI Engineer Singapore Day 3

Companion file for `2026-05-17-ai-engineer-singapore-day-3.md`. Each row is a moment from the Day 3 livestream at a specific timestamp, the section of the blog post it belongs to, what's on screen, and a caption you can use verbatim. Click the timestamp link to jump straight there in YouTube. Suggested filenames assume an `images/` folder next to the post.

Source video: <https://www.youtube.com/watch?v=m12vGjfbNlo>

> **Tip for clean screenshots:** open the video in YouTube's theatre mode, pause exactly on the second, press `F` for fullscreen, then take a screenshot (Cmd+Shift+4 on Mac, Win+Shift+S on Windows). Move the mouse off-screen first so the player chrome fades out.

| # | Blog section | Timestamp | What's on screen | Suggested filename | Suggested caption |
|---|---|---|---|---|---|
| 1 | Opening / hero image | [`7:01`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=421s) | Kaspar Hidayat opening Day 3 — *"Sunday morning, you've chosen sleep deprivation over missing a single second of sessions."* Wide shot of the Capitol Kempinski main stage. | `01-day3-open.jpg` | The host opens Day 3 at the Capitol Kempinski — *"sleep deprivation over missing a single second of sessions."* |
| 2 | Sally Ann — Alyx planning | [`14:00`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=840s) | Slide showing Alyx's planning tools (`todo_write`, `todo_update`, `todo_read`) and the four task states. | `02-alyx-planning.jpg` | Alyx's planning primitives: three tools, four states, and an explicit `finish` gate that blocks the agent from claiming completion. |
| 3 | Sally Ann — context | [`16:30`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=990s) | The `large_json` abstraction / "compress the value, not the structure" slide. | `03-large-json.jpg` | The context-management trick: hard 10K-token tool outputs, JSON skeleton preserved, the full payload retrievable by ID. |
| 4 | Sally Ann — Arize skills | [`22:00`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=1320s) | "Arize skills" slide — Cursor / Claude Code reading Arize traces directly via markdown skills. | `04-arize-skills.jpg` | *"Skills are just markdown. Low cost, high value."* The Arize team's coding-agent loop reads its own production traces. |
| 5 | Cloudflare — code mode | [`39:00`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=2340s) | The TypeScript snippet vs. sequential tool-calls comparison — same task, one turn vs. eight. | `05-code-mode.jpg` | Code mode: the model emits one TypeScript snippet that does loops, branches, and parallel calls — instead of eight sequential tool turns. |
| 6 | Cloudflare — search+execute | [`44:30`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=2670s) | The 2,500 APIs → 1.7M tokens → 1,000 tokens slide. | `06-cloudflare-99pct.jpg` | The full Cloudflare API surface — 2,500 endpoints — fits in 1,000 tokens with search-and-execute code mode. A 99.9% reduction. |
| 7 | Tejas — title card | [`49:00`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=2940s) | Tejas Kumar opening with *"how many of you feel confident you can explain agent harnesses?"* Hands sparse in the room. | `07-tejas-open.jpg` | Tejas Kumar opens with a show of hands. Three people raise theirs. He plans to ask again at the end. |
| 8 | Tejas — six components | [`53:00`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=3180s) | Slide listing the six components of an agent harness (tool registry, language model, context primitives, guardrails, agent loop, verify step). | `08-six-components.jpg` | The skeleton every harness has: tools, model, context primitives, guardrails, agent loop, verify. |
| 9 | Tejas — first run, the lie | [`56:00`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=3360s) | The terminal showing the no-harness run claiming *"I have upvoted the highest-ranked story"* — when in fact the agent hit the login screen and crashed. | `09-the-lie.jpg` | The failure mode harnesses exist to fix: the agent crashes at the login screen and returns *"I have upvoted the highest-ranked story."* An absolute lie. |
| 10 | Tejas — guardrails diff | [`58:30`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=3510s) | Cursor split-view showing the `guardrails.ts` diff being added to the agent loop. | `10-guardrails-diff.jpg` | Step 1: a two-line `guardrails.ts` (max iterations, max messages) wired into the agent loop. The harness layer, not the prompt. |
| 11 | Tejas — verify step | [`1:01:30`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=3690s) | The `verify_successful_upvote` function inspecting the accumulated tool-call list. | `11-verify-step.jpg` | Step 2: the verify step inspects the harness's own log of tool calls — and forces the agent to tell the truth about failing. |
| 12 | Tejas — login handler | [`1:04:00`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=3840s) | The `login_handler.ts` injecting a `harness_auto_login` message into the agent's conversation history. | `12-login-handler.jpg` | Step 3: the harness owns the browser session and logs in invisibly when needed, then injects a message the agent reads as context. |
| 13 | Tejas — the takeaway | [`1:07:00`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=4020s) | Final slide / Tejas delivering the closing line. | `13-tejas-close.jpg` | *"Same prompt. Same old cheap model. If your harness is good, you win 70% of the battle."* |
| 14 | JJ Geewax — title | [`1:32:42`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=5562s) | JJ opening — Applied AI at DeepMind, based in Singapore. | `14-jj-open.jpg` | JJ Geewax, Engineering Director of Applied AI at Google DeepMind — based here in Singapore, and hiring. |
| 15 | JJ — surround with determinism | [`1:44:00`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=6240s) | The route → transform → generate → safety-check architecture diagram. | `15-determinism.jpg` | The architectural pattern: use the LLM as a classifier and a transformer; wrap everything else in deterministic code. |
| 16 | JJ — `temperature=0` is not determinism | [`1:48:00`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=6480s) | The slide / moment where he debunks `temperature=0` as deterministic. | `16-temp-zero.jpg` | *"`temperature=0` is not deterministic. Subtle differences in text mean huge differences in output."* |
| 17 | Geoff Huntley — *Everything is a factory* | [`1:53:33`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=6813s) | Title slide — *"Everything is a Factory"*. | `17-geoff-title.jpg` | Geoff Huntley, back for a second Singapore appearance — *"software development now costs less than minimum wage."* |
| 18 | Geoff — the J curve | [`2:00:00`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=7200s) | The J-curve slide contrasting AI-native startups with incumbents. | `18-j-curve.jpg` | Two classes of companies: AI-native upstarts experimenting with workflow design, and incumbents staring down a three-to-four-year transformation. |
| 19 | Geoff — the hiring filter | [`2:08:00`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=7680s) | The slide on "consumers of AI vs. people who understand AI", with the line dividing the two. | `19-hiring-filter.jpg` | *"It's a curiosity test. Too many engineers are failing."* |
| 20 | Sara Hooker — title | [`5:01:45`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=18105s) | Sara opening her talk after the "everyone stand up and stretch" energiser. | `20-sara-open.jpg` | Sara Hooker, CEO of Adaption Labs, opening with *"typically what drives most frontier research is a feeling that you're very grumpy about something."* |
| 21 | Sara — the cost of static intelligence | [`5:06:00`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=18360s) | The "one-size-fits-all model, everyone does acrobatics around it" slide. | `21-static-intelligence.jpg` | The cost of static intelligence: every user doing acrobatics around the same frozen artifact. |
| 22 | Sara — scaling has flattened | [`5:09:00`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=18540s) | The slide / moment where she lays out that small models are beating much larger ones and 95% of weights can be pruned. | `22-scaling-flat.jpg` | The evidence that brute-force scaling no longer compounds: smaller models winning, 95% prunable weights, disappointing 3-4x scale-ups. |
| 23 | Sara — the era of adaption | [`5:11:30`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=18690s) | The "era of adaption" slide showing the full stack (data → training → inference → interface) as the thing that adapts. | `23-era-of-adaption.jpg` | The era of adaption: the whole stack — data, training, inference, interface — becomes the unit that adapts to each task. |
| 24 | Closing — Rach Pradhan | [`8:56:30`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=32190s) | Rach opening the closing keynote on evolutionary harnesses. | `24-rach-open.jpg` | The day's closing keynote: *"the selection bias itself is the harness."* |
| 25 | Closing — the bitter-lessening | [`9:06:00`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=32760s) | The final slide — *"the artifact worth studying is the path, not the end state."* | `25-bitter-lessening.jpg` | The day's last line: *"this year will be one of the few where you keep seeing the bitter lesson being bitter-lessened."* |
| 26 | Wrap — Agrim and the team | [`9:08:45`](https://www.youtube.com/watch?v=m12vGjfbNlo&t=32925s) | Agrim Singh closing the conference, telling the *"vibe a conference in three months"* story. | `26-wrap.jpg` | Agrim Singh wrapping the conference — *"we're going to YOLO our way into running the biggest conference in town."* It worked. |

## How to slot these into the post

A reasonable distribution for the 20-image-or-so version:

- **Above the title paragraph:** image 1 (hero)
- **End of the orienting paragraph:** image 7 (Tejas opens, to signal where the post is going)
- **Sally Ann section:** images 2, 3, 4 (one per lesson)
- **Cloudflare section:** images 5 and 6
- **Tejas section:** images 8, 9, 10, 11, 12, 13 — this is the centrepiece of the post, so use them all
- **JJ section:** images 14 and 15; image 16 near the `temperature=0` line
- **Geoff section:** images 17 and 18; image 19 near the hiring-filter quote
- **Sara section:** images 20, 21, 22, 23
- **Closing:** images 24 and 25, with image 26 right before the final paragraph

If you want a shorter version, the seven highest-leverage frames are **1** (hero), **7** (Tejas opens), **9** (the lie), **13** (Tejas closes), **15** (JJ's deterministic boundary), **22** (Sara's scaling-flat slide), and **26** (Agrim wrap).

## If you want a shortcut

Several of these speakers post their decks publicly within a few days. Worth searching for Tejas Kumar, JJ Geewax, Sara Hooker, and Geoff Huntley on X / GitHub gists before grabbing YouTube frames — slide exports are always sharper than video screenshots.
