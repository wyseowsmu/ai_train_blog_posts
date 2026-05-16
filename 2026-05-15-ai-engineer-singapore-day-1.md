# AI Engineer Singapore 2026 — Day 1 Notes from the Leadership Track

I spent day one of AI Engineer Singapore at the SMU campus, parked in the Leadership Track. The through-line across almost every session was the same: the model isn't the bottleneck anymore — your operating model is. Here are the talks that stuck with me, what each speaker actually said, and what I'm bringing back to my own team.

## 9:30am — Deploying AI Coding Agents Inside Modern Engineering Organisations

**Nick Miller, Field Engineer, Cursor**

Nick opened by reframing engineering as fleet management. Engineers aren't just writing code anymore — they're directing a swarm of agents and being judged on what the fleet produces. The lines I underlined:

- Specs matter more than ever, because agents need clear direction and boundaries.
- The real bottleneck is verification, not generation.
- Backlog *size* matters less than the *kind* of work being assigned.
- Maintenance should be automated into the workflow, not treated as a separate burden.
- Expect more async collaboration with AI agents — that changes meeting cadence, on-call, and review.

The honest implication is uncomfortable: if your team still measures throughput in tickets closed, you're measuring the wrong thing. Good specs, tests, and observability are now core infrastructure — not nice-to-haves.

## 10:00am — Future Companies and the Software Factory of the Future

**Geoff Huntley**

Geoff's session was the spiciest of the day. His argument: software development has been fundamentally commoditized, and the economics of building software have already changed. Token efficiency, harness choice, and model selection are now first-class engineering decisions.

Some of the lines that landed loudest in the room:

- "If people still haven't had their 'oh f***' moment after two years, they're behind."
- Claude Code is inefficient on token consumption — the harness matters more than people think.
- Software development now costs less than minimum wage when agents run in loops.
- Everyone is now effectively a software developer, even if they're not an engineer.
- Stop hiring on legacy identity signals. Hire on engineering capability.
- If you have no tests, you're screwed.
- Compiler feedback and strong type systems are useful back-pressure against hallucination.
- Open source is less attractive when you can generate first-party code and own the supply chain.

His punchline: durable advantage comes from practice, taste, tooling, and systems that constrain bad output. The bottom of the org is in a "people transformation program." The top is already compounding.

## 11:00am — Turning AI Pilots into Business Impact

**Andy Brown, OpenAI**

Andy's talk was less about models and more about deployment. The framing I liked best: *intelligence is abundant now, the new constraint is deployment*. The productivity era was a stepping stone; the real prize is workflow redesign.

Notes:

- OpenAI is positioning itself as an enterprise company, not just a consumer one.
- Consumer adoption is pulling enterprise adoption forward — people already trust the tools personally.
- Frontier firms aren't winning because they have better models. They're winning because they've embedded AI deep into core workflows — finance, engineering, customer ops, support, sales, decision-making.
- A "Chief of Staff" agent should make you more *effective*, not just *faster*.
- Recursive self-learning and sub-agents make systems materially better.
- Broad access and executive buy-in are the two biggest reasons companies move beyond pilots.
- If leaders don't use the tools themselves, transformation stalls. Every time.

The biggest gains come from embedding intelligence *into* the business, not layering it on top. That distinction is doing a lot of work, and most enterprise AI projects I've seen fail because they got it backwards.

## 11:30am — The Next Wave of Applied AI

**JJ Geewax, Director, Applied AI, Google DeepMind**

This was the most practitioner-flavored session of the day. JJ's argument: weekend projects work, but production systems fail unless they're decomposed and controlled.

The advice I'm taking home:

- Don't shove all logic into the system prompt.
- Use deterministic routing and logging so decisions can be explained later.
- No model is permanent — keep your framework model-agnostic.
- Output filters and verifier models are useful for safety and "wisdom" checks. A verifier model is often better than trying to make one giant prompt do everything.
- Memory is a consolidation-plus-ranking problem. It's not a solved feature.
- Retrieval, memory, and fine-tuning are all useful, but only with clear constraints and good data.
- Fine-tuning is only useful with strong data and a specific target behavior.
- Evals are necessary to know whether the system is good enough.
- Adoption improves when people are allowed to experiment without pressure. Fear of looking behind is a major blocker.

The meta-lesson: production AI is mostly engineering discipline — control, routing, observability, trust. Retrieval, memory, fine-tuning are tools you reach for with intent, not defaults you turn on.

## 12:00pm — More Than the Creator of GLM

**Zixuan Li, Head of Z.ai**

Zixuan pitched GLM as a broad frontier platform and made a strategic case for open weights: security, control, local deployment, and ecosystem diversity. But the more interesting part was his framing of token consumption as a *mindset*.

- GLM started from research and became both a model and a brand.
- Open weights matter because enterprises want security, control, and local deployment. Fine-tuning and adaptation expand model diversity and usefulness.
- The right question isn't "how many tokens did you use?" — it's "what problems did you solve?"
- Token consumption should signal deeper work, more rigorous evaluation, faster experimentation — not be a vanity leaderboard.
- Intelligence is a moving target, not a static score.
- AI-native orgs are defined by how people contribute *context* to AI systems.
- Hierarchy only makes sense when there are real layers of context and access.
- If someone can't contribute useful context, they may not add value in an AI-native setup.

That last point is going to age either really well or really controversially. Either way, it sharpens the question: what *is* the unit of contribution in an AI-native org?

## 2:00pm — Managing Background Agents as an Engineering Workforce

**Ben Lau, Cognition**

Ben picked up the fleet-management thread Nick opened the day with, and pushed it further. His argument: synchronous and asynchronous workflows now coexist, and teams need both.

- Engineers are becoming fleet managers, not just individual contributors.
- Long-running agents work while humans sleep. That alone changes the operating model.
- The right abstraction is no longer just a ticket — it's a well-written spec.
- Industrializing spec production is becoming a competitive advantage.
- Generation is cheap. Verification is scarce. Teams need to *own* testing instead of handing it off.
- Maintenance work — CVEs, CI flakes, dep bumps — is moving from reactive to ambient.
- Senior judgment matters *more*, not less, when agents handle routine work.
- Humans still own accountability. AI should empower them, not replace them.

## 2:30pm — Tracing and Evals with Skills

**Fuad Ali, Arize**

Short and direct: AI is changing software work now, and teams need to adapt rather than wait. Fuad's session was a call to treat internal knowledge as structured, testable context, and to invest hard in observability.

- Product and engineering work will be judged by how well it leverages agents.
- Your knowledge base should be a pristine context layer that agents can consume. Treat internal context as simple markdown-like sources.
- There's no fixed architecture yet — you have to build and test it yourself.
- Skills need evals, or you can't tell if they're good.
- The ecosystem hasn't caught up to large-scale skill testing yet.
- Browser traces and execution traces are how you actually understand agent behavior.
- People should try these systems themselves instead of assuming the answer is obvious.

## What I'm taking home

If I had to compress day one into three sentences:

1. **Verification is the new bottleneck.** Generation has effectively gone to zero. Tests, evals, type systems, and verifier models are the constraint now.
2. **Operating model beats tooling.** The companies pulling ahead aren't the ones with access to better models — they're the ones who've redesigned their workflows around agents.
3. **Specs are the new tickets.** The teams investing in clear specifications and pristine internal context are going to compound. Everyone else will keep generating slop faster.

Day two notes coming after I've had a chance to recover. If you were there and your takeaways differ, I'd love to hear it.
