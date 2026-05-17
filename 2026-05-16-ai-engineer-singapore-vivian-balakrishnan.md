# The Minister Who Codes — Dr Vivian Balakrishnan at AI Engineer Singapore

Day two of AI Engineer Singapore moved off-campus to the Capitol Kempinski, and the keynote everyone in the building had come to see was Dr Vivian Balakrishnan — Minister for Foreign Affairs, former Minister-in-charge of the Smart Nation Initiative, trained ophthalmologist, and (this still feels strange to type) one of the very few sitting government ministers anywhere in the world who actively writes code.

He had set the bar himself a few weeks earlier. On 21 April he had quietly posted, on his own Facebook and a personal GitHub gist, the full technical architecture of his personal AI assistant — a thing called **NanoClaw**, running on a Raspberry Pi in his office, that he uses every day. By the time he walked on stage on Saturday morning, that writeup had been picked up by SCMP, dissected on X, and reposted by half of AI Twitter. The expectations in the room were, fair to say, high.

What we got was not a politician's speech. It was much more interesting than that.

## The headline: stop reaching for the LLM hammer

The line that has been quoted in every write-up since, and rightly so:

> *"We should beware of just trying to throw every problem, and every step in a solution, at a large-language model."*

In a room of 1,000 builders mid-way through the most LLM-soaked event of the year, this was a slightly heretical opening. His point wasn't anti-LLM — he is, manifestly, an LLM enthusiast — it was an engineer's point about **using the right tool for the job**. Rule-based expert systems still work. Deterministic code still works. Traditional, "old-fashioned" AI — the kind programmed step-by-step by humans — still works, and is often faster, cheaper, and easier to debug than an LLM call.

He drove the point home with a comparison nobody else at the conference would have made: the human brain runs on roughly **20 watts**. The data centres training our frontier models run on something closer to the output of a small nuclear plant. Whatever we're doing, he said, **it is clearly not the most efficient possible architecture for thought** — and we should hold that fact in mind every time we reach for an LLM to do something a regex would do.

The broader argument he was making: an "AI system" in 2026 is *not* one giant model. It's a small architecture — a graph of components, some of which are LLMs and most of which aren't. The skill is knowing which piece to use where.

**My take for SMU folks:** this is the unglamorous version of the same lesson Fuad Ali made on Day 1. Most of the value in deploying AI well comes from the *plumbing around the model*, not the model itself. If a vendor is selling you "an AI" as if it's a single magic box, that's a sign they haven't yet built one that works in production.

## The demo: NanoClaw, a "second brain for a diplomat"

The reason the keynote sold out is that Dr Balakrishnan did something almost no public figure ever does: he showed us his actual workflow. Not a sanitised marketing version — the *real* one, with the file paths and the API costs and the warts.

Here's what he runs, in plain English:

- **Hardware: a Raspberry Pi 5.** About **US$80**. Sits in his office. That's the whole machine.
- **Brains: Claude**, accessed via the Anthropic API. **Roughly US$5–20 per month** in usage, at his conversational volume.
- **Memory: a custom knowledge graph** he calls `mnemon` — a SQLite database where every speech he's given, every article he's read, every meeting note, gets broken down into discrete *facts* and stored as nodes with relationships. Not a folder of PDFs. A graph.
- **Inputs: Obsidian on his phone.** He clips articles, dictates voice notes, writes thoughts. They flow into NanoClaw automatically. Voice gets transcribed locally on the Pi using **Whisper**, so the audio never leaves his desk.
- **Interfaces: WhatsApp, Telegram, Slack, Discord, and Gmail.** He talks to it on whichever channel he happens to be on. It talks back the same way.
- **The two intellectual ancestors he cited:** *NanoClaw* itself (the open-source agent harness by Gavriel Cohen, who joined him on stage right after the keynote) and *the LLM Wiki pattern* described by Andrej Karpathy.

The clever bit — the bit he was most pleased with — is how the memory works. He pushed back, gently, on the orthodoxy that says "just throw your documents into a vector DB and do RAG." His system does something different: it **extracts atomic facts** from each document into the graph, then **synthesises those facts into human-readable wiki pages** about each topic, person, and event. When he asks NanoClaw a question, it doesn't just retrieve passages — it retrieves the *synthesised wiki pages* it has built about whatever he's asking about, and injects those as context.

The effect, he said, is that the system gets *smarter about him and his world* the longer he uses it. It learns who he knows, what he's said, what he's read, what he believes. After a year of running, it now writes drafts that sound unmistakably like him — because they are, in a non-trivial sense, written *from* him.

The quote that's been everywhere since:

> *"It answers every question, researches topics, provides daily updates, drafts speeches and condenses information. It has become invaluable — I don't dare switch it off."*

He grinned when he said the last bit. So did the room.

## Why a Foreign Minister built this himself

This was the part of the talk I most wanted senior leadership at SMU to hear.

He was clear that he didn't have to build NanoClaw. He has staff. He has briefings. The Ministry has resources. He built it anyway, on his own time, on his own kit, because — in his words — **you cannot lead a transformation you have never personally experienced.**

He framed it as a duty. If he is going to make decisions, as a minister, about how Singapore engages with AI globally, then he had better understand, at the level of his own fingers on a keyboard, what these systems can and can't do. He doesn't trust the demos. He doesn't trust the vendor pitches. He doesn't trust the breathless press. He trusts the thing he has run himself, in his own office, for a year.

He landed two lines on this, both of which I wrote down:

> *"AI agents have crossed a threshold I did not expect so soon. Not just impressive demos — but practical tools for daily use."*

> *"The diplomat who learns to work with AI will have a meaningful edge. I think that edge is now."*

You can read "diplomat" as "lawyer", "professor", "administrator", "researcher", "manager" — pick your role. The argument is the same.

## On the human role — and why the touch still matters

For all the technical content, the keynote was at heart a humanist talk. He returned, several times, to the limits of what these systems can do. A few of the threads:

- **AI is good at synthesis, bad at judgment under genuine uncertainty.** When the answer to a question is in the training data, the model finds it. When the answer requires a *new* call — a moral one, a political one, a contextual one nobody has made before — the model defers to whoever wrote its system prompt. Diplomats, he noted dryly, are paid for the second kind of call.
- **Tacit knowledge is still the moat.** A career's worth of relationships, read-the-room instincts, knowing when not to speak — none of this is in any model. Some of it can be coaxed into a knowledge graph by patient documentation. Most of it cannot.
- **The human touch in diplomacy is non-negotiable.** A summit is not a transcript. A handshake is not a vector. He was emphatic on this: AI augments the diplomat, it does not replace the diplomat. The same is true, by extension, of doctors, teachers, advisors, leaders.
- **Don't be intimidated.** This was the line he wanted students and civil servants and ordinary employees to hear: the technology has become *easier* to access, not harder. The barrier to entry has collapsed. A Raspberry Pi and an API key. You don't need permission. You need curiosity.

## The five things I want everyone at SMU to take away

If you only remember five things from this post:

1. **An LLM is not the answer to every question.** It's one component in a system. Use it where it adds value; use boring, predictable code everywhere else. The teams who win at AI are the ones who know the difference.

2. **Memory is the real frontier.** The reason NanoClaw feels personal is not the model — it's the slowly-accumulated, well-structured store of facts and synthesised pages *about him* that the model gets to read on every query. A model with no memory is a stranger every morning. A model with the right memory is a colleague.

3. **You learn this by doing it.** A sitting Foreign Minister of a developed country found the time to build his own AI system, on his own Pi, on his own evenings. The single biggest predictor of who is going to thrive in the next five years is not credentials or seniority — it's whether you've actually put your hands on this stuff.

4. **The cost has collapsed.** Eighty US dollars of hardware. Five to twenty dollars a month in API usage. Whatever the perceived barrier to entry in your head is — it is almost certainly lower than that. Pilot it on a personal project before you pilot it at work.

5. **The "moat" is increasingly *your* context, not *their* model.** Everyone has access to the same frontier models. What they don't have is the canonical, well-curated, fact-by-fact record of how *your* institution thinks and works. That asset is built by humans, deliberately, over time. Start building it now.

---

He closed with a thought that I'm still chewing on. Asked from the audience what worried him most about the coming decade, he didn't talk about misalignment, or autonomous weapons, or any of the usual stage-friendly answers. He said the thing he worried about most was **complacency** — that countries, companies, and individuals would assume they had time, when in fact the curve was steeper than it looked from the bottom of it. Singapore, he said, intended not to be complacent. He suggested the rest of us not be either.

Then he walked off stage, sat down next to Gavriel Cohen (who built the NanoClaw harness he uses), and the two of them did a fireside chat about open-source agent design that I'll write up separately.

Come find any of us from the AI Team or IITS if you want to talk about what a Singapore-university version of NanoClaw might look like for our own work. We have some ideas. And, more importantly, a Raspberry Pi.
