# AI Engineer Singapore, Day 3: The Harness Era

This is the writeup I promised at the end of the Day 2 post. Day 3 — Sunday, the last day of AI Engineer Singapore — was the one where a single word kept getting redefined, sharpened, and re-used until it stopped sounding like jargon and started sounding like the actual unit of work everyone in the room has been quietly building toward: the **harness**.

If Day 2's headline was Vivian Balakrishnan's *"tools matter more than models"*, Day 3 was the room collectively underwriting that claim. Speaker after speaker — from Arize's product lead to Google DeepMind's applied AI director to a TypeScript-on-stage live demo from IBM — converged on the same point. The model is the easy part. The interesting engineering is *everything around the model*: the planning state, the guardrails, the verify step, the agent loop, the deterministic boundaries you draw around the non-deterministic centre. That set of things has a name now, and a surprising number of the day's talks were really one long argument about it.

One short orienting note before the recap. The day ran on the Software, Design, and Physical AI tracks at the Capitol Kempinski, with a Demo Stage running in parallel through the afternoon. Some of what ran alongside the segments below, just to give a flavour:

- **9:00am — Timothy Lin (Resaro)** on scenario-specific evals and ODDs for mission-critical AI.
- **2:11pm — Vincent Koc (OpenClaw Foundation)** on the state of OpenClaw.
- **2:31pm — Ben Guo (Zo Computer)** on personal cloud infrastructure and owned agents.
- **2:56pm — Josh Newton (Microsoft AI)** on design as the edge and "taste over slop".
- **3:07pm — Sam Bhagwat (Mastra)** on production agent patterns for customer, internal, and developer workflows.
- **3:25pm — Pierre-Loic Doulcet (LlamaIndex)** on LlamaParse failure modes and parsing at internet scale.
- **5:17pm — Vincent Wu (MiniMax)** on agents that schedule their own compute.
- **5:29pm — Daniel & Siddharth Krishnan (The Robot Company)** on teleoperated robots and closing the autonomy gap.
- **8:31pm — Hrishi Olickel (Southbridge)** on high-context agent runtimes and declarative budgets for legacy systems.
- **8:44pm — Henry Mao (Smithery)** on MCP, CLIs, and "the harness era of agent agency".

…and that's a slice. The [full schedule is here](https://www.ai.engineer/singapore#schedule). This post deliberately picks out the threads of the day that pulled the hardest in one direction — the through-line that the host, Kaspar Hidayat, opened the morning with after thanking the room for picking *"sleep deprivation over missing a single second of sessions"*.

---

## The morning: harnesses, before anyone called them that

### Sally Ann DeLucia — *Lessons from building Alyx*

*Head of Product, Arize*

Sally Ann opened the day with three years of scar tissue from building **Alyx**, Arize's AI engineering agent. She organised the talk around four lessons. Three of them — staying on task, context management, crystallizing good behaviour — were *harness lessons* before the word had been said out loud in the room.

On planning, she was blunt:

> *"Enforcing code, not just prompts. Few-shot examples beat any kind of abstract instructions. Always use the to-do; writing to a plan doesn't work."*

Alyx has explicit `todo_write`, `todo_update`, `todo_read` tools, four task states (`pending`, `completed`, `blocked`, `in_progress`), and a **finish gate** — if Alyx tries to call `finish` without all its to-dos completed, it gets back an *explicit structured error*. Not a nudge. Not a softly-worded suggestion. An error.

She also made a point that stuck with me: **the plan lives outside the conversation history.** It gets injected on every LLM call, right after the system instructions, separate from the truncatable chat log. If the plan can be truncated, the agent can forget what it was doing. That isn't a prompt-engineering problem. That's a state-management problem.

On context, the trick she called out was *compress the value, not the structure*. Alyx's tools never return more than 10,000 tokens; large strings get truncated but the JSON skeleton is preserved, and a `large_json` abstraction stores the full payload behind an ID the agent can fetch on demand. **And** Alyx has access to two general-purpose small tools — `jq` and `gjson` — which it composes the way a Unix programmer composes shell pipelines. The framing she kept coming back to: *think of tools as small Unix utilities and the agent as the shell script.*

The last lesson was a quiet one but important: **vibe-checking does not scale.** Their test cases come from production traces, not handwritten golden answers. Their debug loop is Claude Code and Cursor reading Arize traces directly via "Arize skills" — markdown files that teach the coding agent how to pull observability data and reason about it. Her closing principle:

> *"Skills are just markdown. Low cost, high value. Safety must be wrappers, not prompts."*

The Arize team are eating their own dog food. They are also, whether they used the word or not, building a harness.

### Abhishek Kankani — *Code Mode, or: stop calling tools, write code*

*Emerging Tech & Incubation Lead, Cloudflare*

Abhishek's talk was a tight 12 minutes that did one thing extremely well: it explained why **code mode** is going to eat a lot of what people currently call "MCP tool calling".

The problem statement was crisp. A standard sequential tool-calling agent — list logs, fetch metrics, decide, roll back, etc. — burns context on every turn, because each tool call ships the full conversation history plus the call plus the response back to the model. *"That's bleeding money,"* he said, *"and adding round trips, and adding latency."*

His thesis:

> *"Models are inherently better at writing code than at calling tools — because they've been trained on a ton of code, and most of the tool-calling data they've seen is synthetic."*

Code mode wraps your existing MCP toolkit as **TypeScript type declarations**, passes the model a single tool called `code_mode`, and lets the model emit one TypeScript snippet that does loops, conditionals, parallel calls, variable passing, and branching natively. What used to be five-to-eight sequential turns becomes one. The MCP layer is still there — it's the runtime — the difference is *the model writes code against the toolkit instead of orchestrating it turn-by-turn*.

The line that landed hardest was the Cloudflare-scale stress test. Their full API surface — **2,500 endpoints** — is **1.7 million tokens** if you serialise it as MCP tool definitions, which means it doesn't fit in any model's context. With a *search and execute* code-mode setup — a `search` tool that filters the spec via code, an `execute` tool that runs the call the model picks — they got the entire Cloudflare API callable in **a thousand tokens**. A 99.9% reduction.

> *"I've never seen that level of compression across any sort of things."*

He closed on the question this opens: if you're going to let a model generate and execute arbitrary code, *where* does that code run? Containers are too slow to spin up per request. Cloudflare's answer is **V8 isolates** — the same primitive that backs Cloudflare Workers — which give per-request execution with zero cold start, no secret leakage, and a scoped capability boundary.

That last point is the architectural one. Code mode is fast and cheap. Code mode without a sandbox is the lethal trifecta wearing a TypeScript hat.

### Tejas Kumar — *AI Harnesses From First Principles*

*AI Engineer, IBM*

This is the talk Day 3 will be remembered for, at least by anyone trying to ship an agent in 2026. Tejas — *"pronounced like contagious; don't worry, I'm not"* — flew in from Berlin via Romania to give the most patient and least hand-wavy explanation of what an *agent harness* actually is that I've heard delivered live.

The whole talk was built around one question. He opened by asking the room:

> *"Quick show of hands — how many of you feel confident that you know and can explain agent harnesses?"*

Three people. By the end of the talk, when he asked again, the entire room raised their hands. That alone is worth describing in detail.

**Why we need a harness at all.** The "why" was charmingly literal. You harness a climber to a mountain so they can go up and down *reliably* — that is, without dying. You harness a dog so it stays with you reliably instead of running off. *"The whole point of harnesses for agents or humans or pets or whatever is reliability."* And the reason agents need one is because most of us are sending prompts into someone else's black box and praying for the best.

> *"Has anyone had this sensation? Opus doesn't feel the same today. That's because you trust some foreign body. And this is why we need harnesses."*

**What a harness is, formally.** Tejas's working definition — and the cleanest one I've heard:

> *"An agent harness is everything around your agent — the tool chain, the environment in which your agent executes — that gives it the best chance of success and reliability."*

He enumerated six components every real harness has: a **tool registry**, a **language model**, **context management primitives** (`/compact`, `/clear`, etc.), **guardrails** (quotas, max iterations, max messages), an **agent loop** (the `while-true` that keeps pushing tool calls and messages until a stop condition), and a **verify step** (the *"now run npm run verify"* that closes out the loop). Claude Code, Codex, OpenClaw — every harness in production today has this skeleton or something like it.

**The live build.** And then, instead of more slides, he turned the screen to Cursor and built one. The build is worth describing in detail because it is the kind of thing you can lift wholesale into your own codebase on Monday.

The task: write an agent that goes to Hacker News and upvotes the first story that isn't already upvoted. The constraints he refused to relax: **don't change the prompt** (kept frozen as a deliberately-bad *"upvote a story on Hacker News"*), and **don't change the model** (kept frozen as the cheapest, dumbest, oldest model he could load — GPT-3.5 Turbo). The *only* thing he allowed himself to change was the harness around it.

Run 1, with no harness: the agent opens a Playwright-controlled Chromium, navigates to Hacker News, clicks the upvote, hits the login screen, crashes — and then **lies to the user**, returning *"I have upvoted the highest-ranked story."* Tejas paused on the lie. *"This is an absolute lie."* That is the failure mode harnesses exist to fix.

He built it in three steps, narrating each diff:

1. **Add guardrails.** A two-line `guardrails.ts` with `maxIterations` and `maxMessages`. The agent loop now consults them on every iteration; if either is exceeded, the loop ends with an explicit reason. He also added a brutally-simple `trimContext` that keeps the system prompt, the user prompt, and the most recent two messages. *"There's more intelligent ways to do this. That's not the purpose of this talk."* The point was the *shape*: the guardrails belong in the harness layer, not inside the prompt.

2. **Make the agent tell the truth.** He factored the whole thing into a `run_harness` function and added a `verify_successful_upvote` step that inspects the agent's accumulated tool-call log. If the harness sees a `browser_click` against an element matching `up...`, it returns *upvote click confirmed*. If it sees the agent landed on the login URL without going through the harness's own login tool, it returns `pass: false, reason: "login screen instead of completing upvote"`. Run 2: agent fails, **agent now reports that it failed**, harness reports it failed. *"We did not change the prompt. We did not prompt it harder. And we're still using an old model. But the harness is now giving us some truth."*

3. **Fix it at the harness layer.** He added a `login_handler` — a plain function that does *nothing* unless the browser is on the login page, in which case it fills the username and password directly. Critically, the **browser session is owned by the harness, not the agent**. The handler is wired into the agent loop: if it fires, it injects a message into the conversation history — *"tool name: `harness_auto_login`, result: the harness automatically logged in"* — and the agent reads that and continues. Run 3: the harness logs in invisibly, the agent upvotes the story, the verify step confirms a real click. Same prompt. Same ancient cheap model. *Different harness.*

He landed it with the line the talk had been building toward:

> *"With a harness, you don't need any of this. You can keep the prompt frozen. It can be a bad prompt. You can use an old cheap model. If your harness is good, you win 70% of the battle."*

The closing beat was the one to think about. Today the harness in his demo was hardcoded — he wrote the login handler, the verify step, the guardrails. *"But wouldn't it be amazing if harnesses were dynamic and agents could create their own harnesses and then do work? Dynamic harnesses are likely the next step toward AGI."* That thread was picked up — directly — by the talk that closed the day.

### JJ Geewax — *Surround the non-determinism with determinism*

*Engineering Director, Applied AI, Google DeepMind*

JJ — who also gave a Friday talk at SMU on the same theme — used his Capitol Kempinski slot to push harder on one specific architectural pattern: *don't use the language model as one big router*. He opened with a sincere reminder, which is worth quoting because it's easy to forget while complaining about agents that won't follow instructions:

> *"AI stuff is amazing. Like, it is completely crazy… and there seems to be this world of like, the models are incredible, and they're still at the same time not enough. They don't do real things. Like, my whole job. We're at this sort of weird phase now where we have agents and they do stuff — they call and make restaurant reservations using Eleven Labs and OpenClaw and they're accidentally deleting all our emails. And it's like we're still mad that the agent doesn't follow our instructions."*

That preamble out of the way, the actual architectural point: **the model is being asked to do just a little bit too much.** His team's pattern — and the one he wanted the room to copy — is a flow where the language model is used as a **classifier and a transformer**, and everything else is plain deterministic code. Route → transform (JSON-to-JSON, via Pydantic AI or ADK or Agno) → generate human-facing output → safety check (a smaller, context-free classifier).

> *"I want to use language models for all the hard stuff. I want to use determinism for the stuff that really matters, that I can't compromise on. We can't just tell our customers, don't worry, I added 'don't break any laws' to the prompt. That's not an acceptable answer."*

He also pointed out — and this was new to me — that **`temperature=0` is not determinism**. It's *close to* deterministic, but subtle differences in input text produce huge differences in output. *"It's not like setting a random seed in a pseudo-random number generator."* If your system relies on the model returning the exact same thing twice, you don't have a robust system; you have a coincidence.

His three "walls we bumped into": prompt injection, false determinism, and RAG poisoning (an old refund in your chat history starts handing out refunds; a test row that sold a car for a dollar starts selling cars for a dollar). The fix is the same shape every time: **decompose**. Use small models for the parts that don't need a frontier model. Use deterministic Python around the language-model calls. Use a separate, smaller, context-free model for safety checks so prompt injection in the input can't poison the safety pass.

He left the room with this:

> *"You can't wait for the perfect model. I don't think it'll be here anytime soon. They're good enough now."*

### Geoff Huntley — *Everything is a factory*

*Independent*

Geoff was back for the second time in Singapore — he gave the *"have you had your oh-f**\* moment yet?"* talk at SMU on Day 1 — and his Day 3 slot was less of an inspirational beat and more of a structural one. His central claim: **society has been built around the scarcity of knowledge, and we are now in a knowledge-abundance economy, and the unit economics of every business have therefore forever changed.**

He's been giving this talk in seventeen cities. The one moment that consistently lands is the tour guide in Oakland who, mid-trip to Hobbiton, lights up when Geoff mentions he works in AI and asks: *"Jeff, how good is AI? How good is AI?"*

> *"What does it mean when your tour guide operator is token-maxing? Everyone is now a software developer because AI has enabled everyone to be a software developer, and society has been designed around a scarcity of knowledge. This has changed, folks."*

The professional implications he laid out were less comforting. He pointed at SAP — *"6,800 people doing expense management software, according to LinkedIn"* — and asked the room to think about how long it takes to transform 6,800 people through a J-curve adoption program, versus how long it takes a fifty-person AI-native team to take a chunk of that market. He quoted a New Zealand founder who'd cut their headcount from 60 to 20 by simply refusing to backfill leavers for three years, and was now shipping faster than ever. *"We're not necessarily doing layoffs. They just stopped backfilling."*

His hiring filter, for managers in the room hiring engineers in 2026:

> *"You don't hire on the left of the line anymore. It's a curiosity test… too many engineers are failing. I pull out a whiteboard, they can't explain what a tool call is. They can't show me a sequence diagram of inferencing. They can't talk about the differences in the model cards between vendors. What is temperature?"*

The talk was provocative on purpose — he flagged it himself: *"It's quite a provocative title. Maybe I'm right, maybe I'm wrong."* The bit worth taking seriously, in any case, is the operational one. If an organisation hasn't changed any of its processes, structures, or hiring filters in response to two years of generative AI, that organisation is in the bottom half of the J-curve. Geoff's bet is that the apex predators in the top half — five-person teams running long-running agent fleets — are going to take the next few years' worth of growth.

The connection back to the morning's harness conversation: Geoff is the engineer behind **the Ralph loop**, which is, in his own terms, *"a while loop on a while loop"* — the original viral *"wrap your tool calls in another loop"* pattern that a lot of today's harnesses descend from. He didn't dwell on it on stage, but the lineage was the room's quiet inside joke.

---

## Afternoon: scaling is over, adapt instead

### Sara Hooker — *The future is adaptable*

*CEO, Adaption Labs*

Sara opened the post-lunch keynote with the most grumpy line of the day, in the best sense:

> *"Typically what drives most frontier research is a feeling that you're very grumpy about something and something has to change."*

The thing she's grumpy about is *static intelligence* — the now-orthodox loop of *train the biggest possible model, ship one frozen artifact to everyone, ask users to do prompt-engineering acrobatics around it.* Her thesis, in one breath: the era where doubling model size doubled performance is over, and the field's next decade is going to be about adapting the whole stack — data, training, inference, interface — *on the fly*, per task, per user.

The evidence she walked through is the kind that's hard to argue with even if you disagree with where she lands:

- Small models are now beating much larger ones on the things small models are tuned for.
- You can prune 95% of weights from a trained model and barely move the loss — *"if size were all you need, why are so many weights doing the exact same thing?"*
- The latest 3x–4x scale-ups from frontier labs *"have been seen as not servable and frankly kind of disappointing"* — better on a thin edge, far more expensive on every dimension.
- *"Most of what we gain when we scale is a long tail. So when you double or triple the size of your model, you just learn rare artifacts. That's a very expensive way to learn rare artifacts."*

The bitter lesson, in her framing, *still holds* — but the rate of return on raw scale is no longer the dominant rate of return. Post-training, alignment, data synthesis, adaptive compute, and hardware co-design now compound faster.

> *"The new era of intelligence will require much more than brute force scaling… What matters now is how you leverage capacity, and how you learn from your real-world environment."*

Adaption Labs's two early shots at this are *Adaptive Data* (242 languages, 27 million data points in four weeks — making your data legible to AI fast enough that you can change objectives on the fly) and *Auto Scientist* (a system that, in their internal tests, trained a frontier-tier model across thirty different base architectures in two days, outperforming their own research staff). She's offering Auto Scientist free for a month — *"the proof is in the pudding."*

This is also the talk that quietly justifies the whole "harness" conversation from the morning. If you no longer get to assume the model is the best part of your stack — if the model is just another commodity layer in a system you're adapting around your task — then the *harness* is the part that actually distinguishes one team's product from another's. The hard, defensible engineering is in the layer Tejas was demoing live.

---

## Closing — the harness, evolved

The day ended with a talk by **Rach Pradhan** — independent — on reliable agentic workflows, code intelligence, and parallel agent systems. He took Tejas's "wouldn't it be cool if harnesses were dynamic" prompt from the morning and tried to answer it.

His framing was biological. Energy from the sun hits Earth, life converts it into a particular kind of entropy across three billion years of selection. The *fitness function* in a coding agent, he argued, is the harness — *"the selection bias itself is the harness"* — and harnesses, like organisms, can be evolved rather than written. His own project, **CodeGraph**, was state-of-the-art on Terminal Bench for a while; it lost that crown, but only because *it's the kind of harness that keeps rewriting itself in response to model and benchmark drift*. Around CodeGraph, he'd built a constellation of small tools — `muanry` (a faster ripgrep his agents use for context), `code_db` (a trigram code search for harnesses), `nanobrew` (a faster apt/homebrew so sandbox environments resolve in a fraction of the time), and `dev_swarm` (an orchestrator that mixes Opus and GPT context windows against a rigid source-of-truth benchmark and learns from the telemetry which combinations win).

The thread he closed on, with the day's final line:

> *"The artifact worth studying is the path and the reasoning traces — why a model did something — and not the end state… This year will be one of the few where you keep seeing the bitter lesson being bitter-lessened."*

It was a generous bookend to Tejas's talk eight hours earlier. The morning showed the room what a hand-built harness looks like. The closing showed it what a self-evolving one might.

---

## Three threads worth pulling on

The day had a clear gravitational centre; here are the three threads that seem most worth carrying back into a university context.

1. **The harness is the engineering, not the prompt.** The single biggest mindset shift on the day was *stop arguing with the model; build the scaffolding around it*. A guardrail file, a verify step, a structured `finish` error, a side-channel login handler — these are the things that turn a flaky demo into a reliable workflow. None of them are research; all of them are software engineering. They reward exactly the muscles a university IT team already has.

2. **Determinism is a design choice.** JJ's point and Tejas's point converge here. The model is a non-deterministic centre. Everything around it can — and probably should — be deterministic, testable, and inspectable. `temperature=0` is not the answer. Routing-as-classification, JSON-to-JSON transforms, safety models that don't see the user prompt, and harness-owned resources (browsers, file systems, credentials) are. The interesting engineering is choosing which parts are allowed to be magical and which parts absolutely aren't.

3. **Scale is no longer the moat.** Sara's argument, taken seriously, is that any builder — including any institution — that bet its strategy on "we'll wait for a bigger model" was betting on a curve that has now flattened. The compounding returns are in adapting the surrounding stack — data, harness, tools, interface — to the specific shape of the work being done. For a university, that's a more accessible bet than the previous one. We don't have to train a frontier model. We do have to build the harnesses around the ones that exist.

---

Okay — that's Day 3. The conference ended with Agrim Singh, one of the organisers, telling the story of how this whole thing got vibed into existence in under three months from a lunch conversation in July 2025. Sixty-five labs, a thousand attendees, ninety-one speakers, four parallel tracks, and a closing meditation. The line Vivian used on Day 2 — *"this is all not even their day job. It's a hack, right? But this is the way I believe the future is going to be created."* — was visible in person at the closing.

A few of the talks I didn't cover here — Mastra on production agent patterns, the Robot Company on closing the autonomy gap, Hrishi from Southbridge on declarative budgets for legacy systems, and the design block in the afternoon — are worth their own writeups, and I'll work through them over the coming weeks. If any of the above resonates in the meantime, the AI Team and IITS are around campus. Come find us. We're already arguing about which of these patterns we want to try first.
