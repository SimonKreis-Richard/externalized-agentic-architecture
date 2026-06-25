# The Bounded Agent

> **Design principles for configuring agentic systems under a finite attention budget.**
> For the person hand-writing the YAML and JSON of an open-source agent framework —
> Hermes, OpenClaw, or a custom harness — and trying to decide what goes where.

---

## Who this is for

You are configuring your own agent. You have a `SOUL.md` or a system prompt, a
folder of skills, some memory files, a routing config, and a growing pile of
JSON. Every new capability forces the same decision: *does this go in the core
prompt, a skill file, a memory tier, or nowhere?* Get it wrong and the agent
either drowns in irrelevant context or forgets what it needed.

This framework is a **decision aid** for exactly that situation. It will not
build your agent for you. It gives you the small set of principles that keep the
thousand little configuration choices coherent.

## The premise (borrowed, and credited)

The field has converged on **context engineering** — curating what an agent sees
at each step. Anthropic states the constraint in one line:

> *Good context engineering means finding the smallest possible set of
> high-signal tokens that maximize the likelihood of some desired outcome.*
> — [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents), Anthropic, Sep 2025

This is structural: a transformer's attention is finite, recall degrades as
context grows ("context rot"), and the model spends a real **attention budget**
on every token. We claim none of this. It is transformer physics and cognitive
science, and we credit it as such.

## What's actually ours

The constraint tells you *why* to care. It does not tell you what to do with the
config file open in front of you. Our contribution is two things the
platform-centric literature does not cover:

1. **The Placement Rule** — a single decision procedure that turns the thesis
   into action: for any piece of your agent, decide *where it lives* and *when it
   loads*. The eight principles are its branches. → [`concepts/the-placement-rule.md`](concepts/the-placement-rule.md)
2. **The human in the loop** — we frame the agent as a cognitive architecture
   that includes *you*. Externalizing isn't only context management for the
   model; it's a thinking aid for the person configuring it. → [`concepts/the-reflective-mirror.md`](concepts/the-reflective-mirror.md)

Modest, but real, and useful to the DIY framework user who Anthropic's platform
docs were never written for.

## How each principle is written

- the **problem** it addresses,
- the **principle** itself,
- its **theoretical grounding**, with an honest label (see below),
- a **practical application** — what you actually change in a DIY config (Hermes / OpenClaw / custom),
- a **user-space vs. platform-space** note — must you still build this in 2026, or does the platform do it for you,
- **anti-patterns**.

## A note on sources (read this)

A design framework loses all credibility the moment a reader clicks a citation
and finds it doesn't say what was claimed. So every source carries one of three
labels, held strictly:

| Label | Meaning |
|-------|---------|
| **Validated by** | Direct empirical or practitioner evidence on LLMs/agents. |
| **Consistent with** | A cognitive-science analogy that *orients* design. A fertile metaphor, **not** a proof. |
| **Engineering judgment** | Convergent practice with no specific paper behind it. Stated as such. |

If a principle rests only on judgment, it says so. That honesty is the point.

## Platform-space vs. DIY (the honest stance)

These principles are **increasingly absorbed into the platforms**. Compaction,
file-based memory, and a portable skills format are shipping primitives — e.g.
Anthropic's [memory and context-management tools](https://www.anthropic.com/news/context-management)
and the [Agent Skills format](https://www.claude.com/skills). For most users, a
vertically-integrated turnkey solution will serve these principles better than
anything hand-rolled, because the hard parts are handled under the hood.

We state that plainly. **But** if you are configuring your own agent and want a
principled architecture instead of an ad-hoc pile of prompts, this is the next
best thing. Each principle marks which side of that line it falls on.

## The principles

**Context discipline (single agent)**
1. [Pointer Identity](principles/pointer-identity.md) — identity is a pointer to external structure, not a manual
2. [Progressive Disclosure](principles/progressive-disclosure.md) — load depth on demand, start shallow
3. [Externalized Memory](principles/externalized-memory.md) — durable knowledge lives in files, not context
4. [Context Recycling](principles/context-recycling.md) — compact, distill, promote; never learn the same thing twice

**Orchestration (multiple agents)**
5. [Orchestrated Routing](principles/orchestrated-routing.md) — route each task to an execution mode
6. [Dynamic Delegation](principles/dynamic-delegation.md) — compose workers per task instead of fixing roles

**Governance**
7. [Ariadne's Thread](principles/ariadnes-thread.md) — every decision is traceable to its origin and consequences

**The human-agent loop**
8. [The Reflective Mirror](principles/the-reflective-mirror.md) — the agent externalizes the user's thinking, not only its own

## Structure

```
MANIFESTO.md      → the trade-offs, as value pairs
README.md         → this file
concepts/         → the Placement Rule (the spine) + deeper theory
principles/       → the 8 design principles (theory + DIY practice)
ARCHITECTURE.md   → how the principles compose into a topology
references/        → bibliography, every source verified and labeled
```

## License

MIT — use it, fork it, cite it. Real citations welcome; fabricated ones are the
one thing this repo refuses to ship.
