# Principle: Externalized Memory

> Durable knowledge lives in files, not in context. Context is for reasoning;
> files are for remembering.
> **Family:** Context discipline · **Placement Rule:** Q1 + Q3 · **2026:** part platform-native

---

## Problem

LLMs start every session blank. The naive fix — stuffing facts into the system
prompt — hits three walls: capacity (windows fill), attention (loaded facts
compete with live reasoning), and staleness (facts loaded at the start may be
irrelevant by the end).

## Principle

Route durable knowledge into tiers, by access pattern, and inject only what each
turn needs:

| Tier | Holds | Capacity | Loaded |
|------|-------|----------|--------|
| **Working memory** | Facts that reduce future steering | Hard cap | Every turn |
| **Long-term (skills)** | Repeated procedures | Unlimited | On demand |
| **Episodic (logs)** | Session insights | Unlimited | On search |
| **Reference** | Structured data | Unlimited | On demand |
| **Project** | Per-project state | Unlimited | On context |

The discriminator is Placement Rule Q1 (is it reasoning or content?) followed by
Q3 (what kind of content?).

## Theoretical grounding

- **Validated by** — Anthropic, *Effective context engineering* (2025):
  "just-in-time" agents keep lightweight identifiers and load data at runtime;
  structured note-taking (a `NOTES.md`, a memory tool) persists state "outside of
  the context window" and pulls it back when needed. This "mirrors human
  cognition: we generally don't memorize entire corpuses... we introduce external
  organization and indexing systems."
- **Consistent with** — Clark & Chalmers, *The Extended Mind* (1998): external
  structures that function as cognition *are* part of the cognitive system.
  *(Analogy that justifies treating files as memory, not proof.)*
- **Consistent with** — Cowan (2001): human working memory is ~4±1 chunks, which
  motivates a hard cap on the always-injected tier. *(Analogy.)*

## Practical application (DIY)

- Give working memory a hard character/token cap and enforce it; when full,
  something must be evicted to a lower tier.
- Memory files are **data, not prompts** — no behavioral rules in them; rules are
  skills.
- Keep facts, procedures, and ideas in separate stores. Don't let the idea
  backlog rot inside the fact file.
- Add hygiene: stale facts waste attention, so prune on a schedule.

## User-space vs. platform-space

**Increasingly platform-native.** Anthropic's memory tool and file-based context
management implement the durable-store-outside-context pattern directly. On a
modern harness you configure tiers; on a bare DIY setup you build them. The
*routing discipline* (which tier gets what) stays yours either way.

## Anti-patterns

- **Instructions in memory** — memory is data; behavioral rules belong in skills.
- **Unbounded working memory** — without a cap it grows until it crowds out reasoning.
- **One big bucket** — facts, procedures, and ideas mixed into a single file.

## Sources

| Source | Label | Link |
|--------|-------|------|
| Anthropic — Effective context engineering (2025) | Validated by | https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents |
| Anthropic — Memory & context management tools | Platform reference | https://www.anthropic.com/news/context-management |
| Clark & Chalmers — The Extended Mind (1998) | Consistent with | https://doi.org/10.1093/analys/58.1.7 |
| Cowan — The Magical Number 4 (2001) | Consistent with | https://doi.org/10.1017/S0140525X01003922 |
