# Architecture — How the Principles Compose

> The principles aren't a checklist; they form a topology. This is how they fit
> together around a single agent, across many, and around the human.

---

![Architecture overview](concepts/architecture-overview.svg)

## The one constraint

Everything below exists to answer one question at every step: *what is the
smallest high-signal context that gets this done?* The architecture is just the
set of places content can live so that the context window stays lean.

## Three zones

| Zone | What lives here | Loaded |
|------|-----------------|--------|
| **Context** | Active reasoning, the loaded skill, working memory | Every turn |
| **Durable** | Skills, memory tiers, project context, config | On demand |
| **Archive** | Session logs, learnings, idea backlog | On search |

The flow is a cycle: Archive → Durable (capture / promote) → Context
(load / reason) → Durable (externalize) → Archive. The Placement Rule decides,
for each piece, which zone it belongs to.

## The map (principle → zone → Placement Rule question)

| Principle | Operates on | Placement Rule |
|-----------|-------------|----------------|
| Pointer Identity | the always-loaded core | Q2 |
| Progressive Disclosure | how the periphery loads | Q2 |
| Externalized Memory | the durable tiers | Q1, Q3 |
| Context Recycling | the Context ↔ Archive loop | Q3 |
| Orchestrated Routing | execution mode selection | Q4 |
| Dynamic Delegation | sub-agent context isolation | Q4 |
| Ariadne's Thread | provenance across all zones | Q5 |
| The Reflective Mirror | the human reading the system | Q7 |

## Single agent → many agents → the human

- **One agent:** the context-discipline principles (1–4) keep its window lean.
- **Many agents:** orchestration (5–6) is *distributed* context management —
  isolate expensive context in workers, surface only distilled output.
- **The human:** the same externalization that helps the model (every zone above)
  is a thinking aid for the person configuring it (principle 8).

## User-space vs. platform-space

A 2026 reality check: much of the Durable zone is becoming platform-native —
compaction, file-memory, and skills formats now ship as primitives. The
architecture still holds, but on a modern harness you increasingly *configure*
these zones rather than *build* them. Each principle marks where that line falls.
The decisions — what's core, what tier a fact belongs in, when to delegate, what
deserves a breadcrumb — remain yours.
