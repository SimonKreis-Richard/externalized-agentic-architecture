# Externalized Agentic Architecture

> **Design patterns for context-efficient AI agents.**
> A research-grounded framework for building agents whose cognition extends beyond the context window into durable, external structures.

---

## The Problem

AI agents hit the same wall: **context is finite, cognition is not.**

Every token spent on procedure instructions is a token stolen from reasoning. Every fact loaded into context is a fact that competes for attention. Every skill pre-loaded is a skill that may never be needed.

The result? Agents that are simultaneously over-loaded (too much instruction) and under-informed (too little relevant data).

## The Solution

**Externalize everything that isn't reasoning.**

Identity, memory, procedures, project context — none of these need to live in the context window. They live in files, load on demand, and survive across sessions. The agent's "mind" extends into its filesystem.

This framework codifies 7 patterns discovered through 12+ months of building and operating a production AI agent system. Each pattern is validated by peer-reviewed research and grounded in cognitive science.

## The 7 Patterns

| # | Pattern | Core Insight | Key Source |
|---|---------|-------------|------------|
| 1 | **Pointer Identity** | Agent identity is a pointer, not a manual | Stigmem (2026) |
| 2 | **Two-Layer Loading** | Core always + lazy on-demand = optimal | SRA-Bench (2026) |
| 3 | **Orchestrated Routing** | Route to execution mode, don't pre-assign roles | Dochkina (2026) |
| 4 | **Externalized Memory** | Files > context for durable knowledge | Clark & Chalmers (1998) |
| 5 | **Funnel Cognition** | Ideas → capture → store → harvest | Observed + Extended Mind |
| 6 | **Fil d'Ariane** | Every decision is traceable | Ariadne principle |
| 7 | **Progressive Disclosure** | Load depth on demand, not upfront | Stigmem (2026) |

→ See [`patterns/`](patterns/) for full descriptions with scientific validation.

## Quick Start

1. Read the [`MANIFESTO.md`](MANIFESTO.md) — the philosophical foundation
2. Explore [`patterns/`](patterns/) — the 7 design patterns
3. Read [`ARCHITECTURE.md`](ARCHITECTURE.md) — technical topology and information flow
4. Browse [`concepts/`](concepts/) — deep dives on specific topics

## Who This Is For

- **AI engineers** building production agent systems
- **Researchers** studying agent architecture and cognitive scaffolding
- **Practitioners** who want principled (not ad-hoc) agent design

## Scientific Foundation

This framework draws on:

- **Extended Mind Thesis** (Clark & Chalmers, 1998) — cognition extends into the environment
- **Working Memory Theory** (Cowan, 2001) — 4±1 chunks capacity limit
- **Cognitive Load Theory** (Sweller, 1988) — intrinsic vs extraneous load
- **SRA-Bench** (Su et al., 2026) — 2-layer skill loading validated
- **Stigmem** (Stanford/Eidetic Labs, 2026) — lazy instruction discovery
- **"Don't Break the Cache"** (Lumer et al., 2026) — prompt caching optimization
- **Lost in the Middle** (Liu et al., 2023) — U-shaped attention bias
- **Dynamic Delegation** (Dochkina, 2026) — orchestrator routing > fixed roles

→ See [`references/bibliography.md`](references/bibliography.md) for full citations.

## Status

**Active development.** This framework is extracted from a production system operating since May 2026.

## License

MIT — use it, fork it, cite it.

---

*Built by a practitioner who discovered these patterns the hard way — through 6 SOUL rewrites, 15K-token mistakes, and the humbling realization that the simplest architecture is usually the right one.*
