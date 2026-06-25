# Principle: Ariadne's Thread (Traceability)

> Every externalized decision should be traceable to its origin and forward to
> its consequences.
> **Family:** Governance · **Placement Rule:** Q5 · **2026:** user-space

---

## Problem

Externalization fragments knowledge across many files. Without a thread back to
why each fragment exists, the system rots in a specific way: knowledge becomes
orphaned (no one knows why a file is there), decisions become opaque (no
rationale survives), and debugging becomes guesswork (you can't trace an error to
its source).

## Principle

Every durable artifact carries provenance, and the trace runs both ways:

- **Backward:** this file/skill/decision exists *because of* this need or
  conversation.
- **Forward:** this decision *led to* these consequences.

| Artifact | Trace |
|----------|-------|
| Files | created when, from which session |
| Decisions | rationale + alternatives considered |
| Skills | the recurring need that justified promotion |
| Config changes | a diff with rationale and blast radius |

## Theoretical grounding

- **Engineering judgment** — this is software-engineering discipline, not an LLM
  research finding, and it's labeled as such. Version control already provides
  the mechanism: a commit is a breadcrumb with a timestamp and a reason.
- **Consistent with** — audit-trail requirements in regulated work (e.g. the
  spirit of ISO 27001's logging expectations): traceable beats complete when
  trust is at stake. *(Borrowed rationale, not a claim about agents specifically.)*

The name is the metaphor: Ariadne's thread let Theseus navigate the labyrinth and
find his way back. Here the labyrinth is your own externalized system.

## Practical application (DIY)

- Commit config and skill changes with real messages — the *why*, not just the
  *what*. Your git log is the cheapest traceability you'll ever get.
- Put a short provenance header on durable files (created, source, purpose).
- When you discard an idea, record the reason — a trace, not a regret.

## User-space vs. platform-space

**Yours.** No platform decides what's worth a breadcrumb; this is a habit you
build into how you configure and commit.

## Anti-patterns

- **Orphan knowledge** — files with no provenance; nobody remembers why they exist.
- **Silent changes** — editing config without recording the rationale.
- **Trace nobody can reach** — provenance stored where it's never read.

## Sources

| Source | Label | Link |
|--------|-------|------|
| Git (version control as provenance) | Engineering judgment | https://git-scm.com/ |
