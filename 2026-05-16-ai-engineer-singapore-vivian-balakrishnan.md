# AI Engineer Singapore, Day 2: The Forward Deployed Minister

This is the writeup I promised at the end of the Day 1 post. The keynote everyone in the building had come to see was Dr Vivian Balakrishnan — Minister for Foreign Affairs, former Minister-in-charge of the Smart Nation Initiative, retired eye surgeon, and (this still feels strange to type) one of the very few sitting government ministers anywhere in the world who actively writes code.

Someone in the coffee queue afterwards called him a *"forward deployed minister"* — a riff on the **forward deployed engineer**, the now-fashionable role for engineers who embed directly with customers, build prototypes in the field, and refuse to be insulated from the actual work. It is exactly the right framing for what he did on stage. He wasn't there as a politician describing AI from the outside. He was there as a practitioner, deployed into his own workflow, showing the room what he had built with his own hands.

He had set the bar himself a few weeks earlier by quietly posting the full architecture of his personal AI assistant — **NanoClaw**, running on a Raspberry Pi in his office — on his own Facebook, with a [technical writeup as a GitHub gist](https://gist.github.com/VivianBalakrishnan/a7d4eec3833baee4971a0ee54b08f322) describing the stack in full. By the time he walked on stage at the Capitol Theatre, that writeup had been picked up by SCMP, dissected on X, and reposted by half of AI Twitter. The expectations in the room were, fair to say, high.

What we got was not a politician's speech. It was much more interesting than that. The full session is on YouTube (his segment begins around the 45-minute mark) and what's below is built directly off the transcript.

One quick orienting note for context. The minister's session — officially titled *["Building a 'Second Brain': Opportunities, Risks, and Implications for AI Adoption in Singapore"](https://www.ai.engineer/singapore#schedule)* — was the **8:40am Day 2 opener** on the Software track, but it was one slot in a relentlessly packed day. Some of what ran in the next eight hours, just to give a flavour:

- **9:00am — Gavriel Cohen (NanoCo)**, the creator of the NanoClaw harness the minister had just spent twenty minutes demoing, talking about NanoClaw's own internal agent factory.
- **9:18am — Thibault Sottiaux**, Head of Codex at **OpenAI**, on amplifying every builder with agents.
- **9:41am — Dr Feng Yuzhang (GovTech Singapore)** on AI transformation across the Singapore government.
- **10:40am — Jimmy Lai (Vercel)** on shipping with Next.js.
- **11:03am — Vedran Jukic (Daytona)** on why sandboxes are non-negotiable for autonomous agents.
- **11:16am — Greptile** on what they learned from analysing five million vibe-coded PRs.
- **12:08pm — Mark Doyle (Stripe)** on Stripe's one-shot end-to-end coding agents.
- **1:39pm — Ryo Lu**, Head of Design at **Cursor**, on designing the next Cursor.
- **2:02pm — Aosheng Ran (Figma)** on designing multi-modal, multiplayer AI.
- **2:54pm — Andrew Tan (Groq)** on scaling low-latency LLM inference.
- **3:07pm — Daria Soboleva (Cerebras)** on MoE at wafer scale.
- **3:35pm — Zixuan Li (Z.ai)** on GLM-5.1 and long-horizon tasks.
- **3:58pm — Boris Starkov (ElevenLabs)** on what makes an agent conversational.

…and that's just a slice. Four parallel tracks (Software, Design, Physical AI, and a Demo Stage) ran in parallel through the day. The [full schedule is here](https://www.ai.engineer/singapore#schedule). This post deliberately covers only the minister's segment — the rest will get their own writeups.

He opened by introducing himself with characteristic dryness: *"I'm actually a retired eye surgeon. Took a detour into politics for perhaps too long… I assemble watches, I reprogram appliances, and now there's some other stuff which is what I'm going to talk about today."* And then, unusually for a keynote, he jumped straight to the end and stated his three takeaways upfront — *"you can forget everything I've said but just bear these things in mind."* The whole rest of the talk hung off those three.

## Takeaway 1 — You cannot outsource your personal understanding

> *"We're now at an age when you can outsource a lot of stuff — calculations, computation, memory, replication, dissemination of knowledge. The one thing which you cannot outsource is your personal understanding. And if you are in a position of authority, you can delegate work. You can't delegate accountability."*

The point isn't anti-AI — he is plainly an AI enthusiast. The point is that the *output* of an AI-augmented workflow still has to be owned by a human being who genuinely understands it, especially if that human is the one signing things, deciding things, or being held to account for them.

Later in the talk he pushed this further into a line he attributed (somewhat suspiciously) to Claude, but said he agreed with anyway:

> *"You cannot govern a technology that you have only been briefed on. You'd better get your hands dirty — and then you understand both the potential and the limits and the problems."*

He explicitly framed that as a shoutout to his government colleagues. The same logic generalises easily to any institution where decisions are made about a technology by people who have never personally used it.

## Takeaway 2 — Real value is created workflow by workflow, not at the frontier

He pointed the audience at a recent Financial Times letter by **Professor Neil Lawrence** (University of Cambridge, machine learning), and summarised the argument like this:

> *"There's a lot of hype about AI models, data centres, top-down systems, rules, governments. That's macro. But his hypothesis is that real value for the economy and society is created at the ground level — workflow by workflow, sector by sector, department by department, and in fact at the individual level."*

His own elaboration:

> *"I know you guys are great and I know the guys working on frontier models are incredible, but the real payoff is when ordinary people — teachers, lawyers, technicians, managers, doctors, or even ministers — are actually using the tools which are already available, already invented. People who know their jobs and are empowered by these tools. That's how you create real value for society and for the economy."*

He kept returning to three words: **decentralisation, individualisation, bespoke**. The future he was describing is not one giant model serving humanity from a data centre. It is millions of small, personal, idiosyncratic systems — one per knowledge worker — each fitted to that person's actual job.

## Takeaway 3 — The barriers have collapsed

> *"I sincerely believe the barriers for achieving all this have collapsed. The tools have already been availed. It's a matter of getting people to understand what tools are out there, assemble their own tools, and put ourselves on a completely different trajectory."*

The proof of that claim was the rest of the talk: a walk-through of his own setup, built by a sitting Foreign Minister with no formal software background, on parts that anyone in the room could buy or download for themselves.

## The build itself: NanoClaw, mnemon, and a three-year-old Raspberry Pi

Here is, in his own framing, the stack he actually runs every day (cross-checked against his [published gist](https://gist.github.com/VivianBalakrishnan/a7d4eec3833baee4971a0ee54b08f322)):

- **Hardware.** A **Raspberry Pi 5** (aarch64), 8 GB of RAM. *"My most daily-used agent is running off a Raspberry Pi which is at least two or three years [old]."* That's it. That's the machine.
- **Agent harness: [NanoClaw](https://github.com/qwibitai/nanoclaw)**, by **Gavriel Cohen** (CEO of NanoCo, who took the stage at 9am right after the minister to talk about NanoClaw's own internal agent factory). A Node.js + TypeScript orchestrator that runs the **Claude Agent SDK** inside isolated **Docker containers**. He had originally been excited by the broader hype around "open-claw"-style agents but ruled them out for security reasons given his role. NanoClaw won him over because the codebase is short enough that *"even an idiot like me can read and sort of understand [it]"*, it's containerised (the surgeon in him spoke up here: *"there's no such thing as a routine operation and things will go wrong, and when they do break, hopefully you want them to break within barriers"*), and it has effectively no config — the LLM handles the customisation, which means *"everyone running an instance of NanoClaw is running an individualised system."*
- **Brains:** Claude, via the Claude Agent SDK (he was clear NanoClaw v2 should treat *all* major models as first-class citizens — he's asked Gavriel for this by 15 June).
- **Credentialing: OneCLI**, a proxy that means the containers never see raw API keys — important given his job, and one of the things he specifically called out as not having had to write himself.
- **WhatsApp bridge: Baileys.** *"I suspect it's probably not entirely in keeping with what Meta or WhatsApp would like us to do, because it's actually simulating… a pseudo-terminal."* He says this with a small smile.
- **Voice: whisper.cpp** running on-device — so the audio never leaves the Pi.
- **Memory: mnemon** — *"this obscure piece of software… I still haven't met the developers… a memory system with graphs."* A SQLite-backed graph database where each entry has content, category, importance score, tags, timestamp, and edges to related entries.
- **Semantic search:** **Ollama** running locally with the **nomic-embed-text** embedding model — so memory recall isn't limited to keyword match, and no embedding traffic leaves the Pi either.
- **Synthesis: Andrej Karpathy's "LLM-supervised wiki generation" pattern**, layered on top of the memory graph — the system doesn't just retrieve facts, it synthesises wiki pages (organised into `entities/`, `concepts/`, and `timelines/`) about people, countries, topics, that get re-read on every relevant query.
- **UX: Obsidian**, syncing through Apple iCloud, which gives him a "personal cloud" of curated material and generated wiki pages he can pull up anywhere.
- **And — a small detail he dropped almost in passing —** the slides for this very keynote were generated by Claude.

The job it does for him, in his words:

> *"This month I'm visiting 12 countries… I will have to meet hundreds of people. I will have to understand each country's economy, geography, culture, history, war and peace. I need to know people as individuals and not just from a brief. There's a huge cognitive overload on every single diplomat. The question is how can I turbocharge this process — so that if I need a fact or a factoid, I can get it anywhere, and I can go down the rabbit hole if need be."*

And on whether it actually works, the line that everyone has been quoting since:

> *"I have not dared to switch it off."*

He delivered it deadpan. The room laughed. He also let slip that NanoClaw v1 is still running on one machine while he tries v2 on another, *"because the transition is not at all smooth"* — which is exactly the kind of detail you only get from a person who has genuinely been using a thing in production, rather than presenting one.

He was also pointedly modest about the engineering:

> *"You know, there's this whole thing about vibe coding. I won't even dare to claim I was vibe coding. I was just assembling tools."*

He was also clear that *reading* code matters even when you aren't writing it. He reviews the code NanoClaw asks him to approve before granting bash access. *"It does help if you don't understand coding, so you understand what's going on, even if you're not actually typing and editing code in the raw."*

## "For a man with a hammer, everything looks like a nail"

The second half of the talk pulled in the opposite direction from the demo: an explicit warning *against* over-using LLMs.

> *"We should beware of just trying to throw every problem, and every step in a solution, at an LLM. It reminds me of the old proverb — for a man with a hammer, everything looks like a nail."*

His reasoning was an unusual mix of economics and biology. On the economics:

> *"Tokens are not cheap. Compute power is limited. Electricity prices have risen. Wars do not help. The prices the AI majors are currently charging us — I think we all know we're enjoying, in effect, a subsidy."*

On the biology — and this is where being an eye surgeon shows up:

> *"As a biologist, [I think] in the end some kind of neuro-symbolic system, rather than just the LLM model… I have some sympathy for Yann LeCun, who says LLMs are great, but actually that's not the way we've solved it in nature. If you look at the human brain, I suspect we have fewer layers of computation in the human brain than in many of the large language models which we have today. And I can tell you as an eye surgeon, the cortical computation for vision, for language, for cognition are often based on far more efficient structures than the energy-gobbling systems which we have today."*

The practical implication: *"There is still a role for deterministic systems. There is still a role for expert rule-based systems."* Use the LLM where it adds value. Use boring, predictable, *cheap* code everywhere else.

And the line he wanted to leave the engineers with:

> *"Tools matter more than models."*

## Memory, and the unsolved frontier

If there is one technical theme he kept returning to, it was memory.

> *"Memory — it is a very human, and I think it is the great unsolved part of this frontier."*

This is why he built around mnemon + Karpathy's wiki pattern rather than the more common "dump documents into a vector DB and do RAG" approach. His system extracts atomic facts from each ingested document into a graph; then synthesises *wiki pages* about each entity from those facts; then retrieves those synthesised pages as context on every query. The effect, he said, is that the system slowly accumulates an actual model of *his world* — who he knows, what he's said, what he's read — rather than just a searchable haystack.

This is a useful conceptual reframe even for people who will never touch a graph database. **Most of what makes an AI assistant feel useful is not the model — it's the structured, accumulated, well-curated record of you and your work that the model gets to read every time you ask it something.** Build the record, and the model becomes a colleague. Skip the record, and the model is a stranger every morning.

## On security — and an unusual answer

A predictable concern at a talk by a sitting Foreign Minister: what happens if his system is compromised? His answer was characteristically dry:

> *"Even if you hack my system, the most you'll get from it is my phone number. You will get summaries of foreign policy — but since it's foreign policy which I have espoused, and in any case I have curated the stuff I've put in… even if you take my system, I think it will generate the foreign policy of Singapore anyway."*

The serious point underneath the joke: *one way* of addressing AI security is to deliberately only put already-open-source, already-published material into the system in the first place — and then *subject your systems to a level of transparency and scrutiny that can be withstood*. Not the only way, and not appropriate for every context, but a useful framing for any team thinking about what data to let an AI touch.

He did, however, flag the broader political picture — *"commercial competition, national security, cybersecurity, and superpower contestation"* — as the factors that will most shape the availability and speed of AI globally. That, he noted, is a separate political talk well worth a deep dive on another day.

## Closing — democratisation, the edge, and a hack

He ended on a Singapore-policy note that landed clearly in the room. Quoting the Economic Strategy Review Committee:

> *"Singapore is not likely to be at the frontier of model development. But we can be at the frontier of deployment at scale."*

And his own framing on top of that:

> *"I'm a believer in deployment at the edge. I'm a surgeon. I believe in doing. I believe in fixing. I believe that's where lives are saved and value is created."*

That line, more than any other, is what makes the "forward deployed minister" joke work. The whole talk is an argument that the people creating real value with AI are the ones embedded in actual workflows — doing, fixing, deploying — rather than the ones theorising about it from a distance. He was modelling the behaviour he wanted his audience, his civil servants, and his counterparts in other capitals to adopt.

The public-policy goal, in his words, is *democratisation of these tools* — and therefore *a decentralised, ground-up approach*. He then gestured at the room and ended on the conference itself:

> *"This conference was organised less than three months ago. 65 labs. All the people you meet here — this is all not even their day job. It's a hack, right? But this is the way I believe the future is going to be created."*

Then he walked off stage, and the 9am slot belonged to **Gavriel Cohen** — CEO of NanoCo and the creator of the harness the minister had just spent twenty minutes demoing — who I'll write up separately.

---

## Three threads worth pulling on

The talk had a clean three-part structure of its own; here are the three threads that seem most worth carrying back into a university context.

1. **Accountability doesn't get delegated.** AI can draft a report, prep a meeting, summarise an inbox, build a deck. None of that changes who owns the output. The minister's "personal understanding" argument is essentially a working definition of what an AI-augmented professional looks like: someone who can read what their tools produced, recognise where it's right and where it isn't, and put their name on it deliberately.

2. **Value lives in workflows, not in models.** Everyone has access to the same frontier models. The institutions that pull ahead seem to be the ones doing the unsexy work of *re-engineering an actual workflow* — a course, an admissions process, a finance close, a research review — around what AI can now do. Models are a commodity. Workflows are not.

3. **The barrier to entry is genuinely low now.** A retired eye surgeon, in spare time, on a three-year-old Raspberry Pi with 8 GB of RAM, built a system he uses every day to do a globally consequential job. Whatever the old story was about why this stuff is for someone else — it has aged badly.

If any of the above resonates and you want to compare notes, the AI Team and IITS are around. We also, as it happens, have a Raspberry Pi.
