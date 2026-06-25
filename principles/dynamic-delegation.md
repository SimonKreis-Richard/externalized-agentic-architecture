# Principle: Dynamic Delegation

> When you do delegate, compose the worker for the task — and have it return a
> distilled summary, not its full context.
> **Family:** Orchestration · **Placement Rule:** Q4 · **2026:** part platform-native

---

## Problem

Once routing decides a task needs parallel work, a second trap appears: handing
each worker the whole context, and pulling back everything it produced. That
defeats the purpose — you've just multiplied context pollution across several
agents instead of one.

## Principle

Two rules for delegation:

1. **Compose per task.** Spin up a worker scoped to the sub-task, with a clean
   context window and only the inputs it needs — rather than a standing,
   general-purpose role.
2. **Return distilled.** The worker may explore widely (tens of thousands of
   tokens), but it returns a condensed result (often ~1–2K tokens). The detail
   stays isolated in the worker; the lead agent receives the conclusion.

This is the orchestration form of the attention-budget thesis: isolate the
expensive context, surface only the high-signal output.

## Theoretical grounding

- **Validated by** — Anthropic, *How we built our multi-agent research system*
  (2025): a lead agent coordinating specialized sub-agents, each returning a
  distilled summary, showed "a substantial improvement over single-agent systems
  on complex research tasks."
- **Validated by** — Anthropic, *Effective context engineering* (2025): sub-agent
  architectures keep "detailed search context... isolated within sub-agents,
  while the lead agent focuses on synthesizing."
- **Engineering judgment** — the choice between compaction, note-taking, and
  sub-agents is task-dependent; delegation pays off specifically where parallel
  exploration does.

> *Note: an earlier draft cited a fabricated "Dynamite Delegation (Do, 2024),
> 38.7% on GAIA." It does not exist and has been removed.*

## Practical application (DIY)

- Give each spawned worker a narrow brief and a clean window — not your whole
  `SOUL.md` and history.
- Constrain fan-out (e.g. max spawn depth and max concurrency) so coordination
  cost stays bounded.
- Require workers to return a summary in a fixed shape; never merge a worker's
  raw transcript back into the lead context.

## User-space vs. platform-space

**Both.** Sub-agent primitives increasingly ship with harnesses; the *delegation
policy* — scoping, fan-out limits, summary contracts — is your configuration.

## Anti-patterns

- **Fat hand-offs** — passing full context to workers and merging raw output back.
- **Standing roles** — long-lived general workers instead of task-scoped ones.
- **Unbounded fan-out** — spawning without depth/concurrency limits.

## Sources

| Source | Label | Link |
|--------|-------|------|
| Anthropic — Multi-agent research system (2025) | Validated by | https://www.anthropic.com/engineering/multi-agent-research-system |
| Anthropic — Effective context engineering (2025) | Validated by | https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents |
