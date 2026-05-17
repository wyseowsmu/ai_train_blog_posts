# The Minister Who Codes — Dr Vivian Balakrishnan at AI Engineer Singapore

This is the writeup I promised at the end of the Day 1 post. The keynote everyone in the building had come to see was Dr Vivian Balakrishnan — Minister for Foreign Affairs, former Minister-in-charge of the Smart Nation Initiative, retired eye surgeon, and (this still feels strange to type) one of the very few sitting government ministers anywhere in the world who actively writes code.

He had set the bar himself a few weeks earlier by quietly posting the full architecture of his personal AI assistant — **NanoClaw**, running on a Raspberry Pi in his office — on his own Facebook. By the time he walked on stage at the Capitol Theatre, that writeup had been picked up by SCMP, dissected on X, and reposted by half of AI Twitter. The expectations in the room were, fair to say, high.

What we got was not a politician's speech. It was much more interesting than that. The full session is on YouTube (his segment begins around the 45-minute mark) and what's below is built directly off the transcript.

He opened by introducing himself with characteristic dryness: *"I'm actually a retired eye surgeon. Took a detour into politics for perhaps too long… I assemble watches, I reprogram appliances, and now there's some other stuff which is what I'm going to talk about today."* And then, unusually for a keynote, he jumped straight to the end and stated his three takeaways upfront — *"you can forget everything I've said but just bear these things in mind."* The whole rest of the talk hung off those three.

## Takeaway 1 — You cannot outsource your personal understanding

> *"We're now at an age when you can outsource a lot of stuff — calculations, computation, memory, replication, dissemination of knowledge. The one thing which you cannot outsource is your personal understanding. And if you are in a position of authority, you can delegate work. You can't delegate accountability."*

This is the line I most want senior leadership at SMU to hear. The point is not anti-AI — he is plainly an AI enthusiast. The point is that the *output* of an AI-augmented workflow still has to be owned by a human being who genuinely understands it. Especially if that human is the one signing things, deciding things, or being held to account for them.

Later in the talk he pushed this further into a line he attributed (somewhat suspiciously) to Claude, but said he agreed with anyway:

> *"You cannot govern a technology that you have only been briefed on. You'd better get your hands dirty — and then you understand both the potential and the limits and the problems."*

He explicitly framed that as a shoutout to his government colleagues. It applies just as well to deans, directors, and department heads.

## Takeaway 2 — Real value is created workflow by workflow, not at the frontier

He pointed the audience at a recent Financial Times letter by **Professor Neil Lawrence** (University of Cambridge, machine learning), and summarised the argument like this:

> *"There's a lot of hype about AI models, data centres, top-down systems, rules, governments. That's macro. But his hypothesis is that real value for the economy and society is created at the ground level — workflow by workflow, sector by sector, department by department, and in fact at the individual level."*

His own elaboration:

> *"I know you guys are great and I know the guys working on frontier models are incredible, but the real payoff is when ordinary people — teachers, lawyers, technicians, managers, doctors, or even ministers — are actually using the tools which are already available, already invented. People who know their jobs and are empowered by these tools. That's how you create real value for society and for the economy."*

He kept returning to three words: **decentralisation, individualisation, bespoke**. The future he was describing is not one giant model serving humanity from a data centre. It is millions of small, personal, idiosyncratic systems — one per knowledge worker — each fitted to that person's actual job.

## Takeaway 3 — The barriers have collapsed

> *"I sincerely believe the barriers for achieving all this have collapsed. The tools have already been availed. It's a matter of getting people to understand what tools are out there, assemble their own tools, and put ourselves on a completely different trajectory."*

The proof of that claim was the rest of the talk: a walk-through of his own setup, built by a sitting Foreign Minister with no formal software background, on parts that anyone in the room could buy or download for themselves.

## The build itself: NanoClaw, Neman, and a three-year-old Raspberry Pi

Here is, in his own framing, the stack he actually runs every day:

- **Hardware.** *"My most daily-used agent is running off a Raspberry Pi which is at least two or three years [old]. All it has is 8 GB of RAM."* That's it. That's the machine.
- **Agent harness: NanoClaw**, by **Gabriel Cohen** (who took the stage right after him). He had originally been excited by the broader hype around "open-claw"-style agents but ruled them out for security reasons given his role. NanoClaw won him over because the codebase is short enough that *"even an idiot like me can read and sort of understand [it]"*, it's containerised (the surgeon in him spoke up here: *"there's no such thing as a routine operation and things will go wrong, and when they do break, hopefully you want them to break within barriers"*), and it has effectively no config — the LLM handles the customisation, which means *"everyone running an instance of NanoClaw is running an individualised system."*
- **Brains:** Claude (he was clear NanoClaw v2 should treat *all* major models as first-class citizens — he's asked Gabriel for this by 15 June).
- **WhatsApp bridge: Baileys.** *"I suspect it's probably not entirely in keeping with what Meta or WhatsApp would like us to do, because it's actually simulating… a pseudo-terminal."* He says this with a small smile.
- **Voice: Whisper**, so he can talk to it rather than type, and it talks back.
- **Memory: Neman** — *"this obscure piece of software… I still haven't met the developers… a memory system with graphs."* Entities as nodes, edges for causality, temporal relationships, semantic links.
- **Semantic search: Ollama running locally** with an embedding model, so memory recall isn't limited to keyword match.
- **Synthesis: Andrej Karpathy's "LLM-supervised wiki generation" pattern**, layered on top of the memory graph — i.e. the system doesn't just retrieve facts, it synthesises wiki pages about people, countries, topics, that get re-read on every relevant query.
- **UX: Obsidian**, syncing through Apple iCloud, which gives him a "personal cloud" of curated material and generated wiki pages he can pull up anywhere.
- **And — a small detail he dropped almost in passing —** the slides for this very keynote were generated by Claude.

The job it does for him, in his words:

> *"This month I'm visiting 12 countries… I will have to meet hundreds of people. I will have to understand each country's economy, geography, culture, history, war and peace. I need to know people as individuals and not just from a brief. There's a huge cognitive overload on every single diplomat. The question is how can I turbocharge this process — so that if I need a fact or a factoid, I can get it anywhere, and I can go down the rabbit hole if need be."*

And on whether it actually works, the line that everyone has been quoting since:

> *"I have not dared to switch it off."*

He delivered it deadpan. The room laughed. He also let slip that NanoClaw v1 is still running on one machine while he tries v2 on another, *"because the transition is not at all smooth"* — which is exactly the kind of detail you only get from a person who has genuinely been using a thing in production, rather than presenting one.

He was also pointedly modest about the engineering:

> *"You know, there's this whole thing about vibe coding. I won't even dare to claim I was vibe coding. I was just assembling tools."*

But — and this is the part I want non-engineers at SMU to hear — he is clear that *reading* code matters even when you aren't writing it. He reviews the code NanoClaw asks him to approve before granting bash access. *"It does help if you don't understand coding, so you understand what's going on, even if you're not actually typing and editing code in the raw."*

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

This is why he built around Neman + Karpathy's wiki pattern rather than the more common "dump documents into a vector DB and do RAG" approach. His system extracts atomic facts from each ingested document into a graph; then synthesises *wiki pages* about each entity from those facts; then retrieves those synthesised pages as context on every query. The effect, he said, is that the system slowly accumulates an actual model of *his world* — who he knows, what he's said, what he's read — rather than just a searchable haystack.

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

The public-policy goal, in his words, is *democratisation of these tools* — and therefore *a decentralised, ground-up approach*. He then gestured at the room and ended on the conference itself:

> *"This conference was organised less than three months ago. 65 labs. All the people you meet here — this is all not even their day job. It's a hack, right? But this is the way I believe the future is going to be created."*

Then he walked off stage, sat down next to **Gabriel Cohen**, the creator of NanoClaw, who I'll write up separately.

---

## Three things I want everyone at SMU to take away

I'll try to mirror his structure here. If you only remember three things:

1. **You cannot delegate accountability.** AI can draft your report, prep your meeting, summarise your inbox, build your slides. None of that removes the requirement that *you* understand the thing you are putting your name on. The minister's whole point about "personal understanding" is the most senior-leadership-flavoured argument I've heard for actually using these tools yourself rather than receiving demos of them.

2. **Value lives in workflows, not in models.** Everyone has access to the same frontier models. What separates the institutions that pull ahead from the ones that don't is whether they have done the unsexy work of *re-engineering an actual workflow* — a course, an admissions process, a finance close, a research review — around what AI can now do. Start with one workflow you know intimately. That's where your edge is.

3. **The barrier to entry is now embarrassingly low.** A retired eye surgeon, in spare time, on a three-year-old Raspberry Pi with 8 GB of RAM, built a system he uses every day to do a globally consequential job. Whatever you have been telling yourself about why you can't start — it's almost certainly wrong. The minister's closing instruction, with his blessing as a senior official: get your hands dirty.

Come find any of us from the AI Team or IITS if you want to talk about what a Singapore-university version of this might look like for our own work. We have some ideas. And, more importantly, a Raspberry Pi.
