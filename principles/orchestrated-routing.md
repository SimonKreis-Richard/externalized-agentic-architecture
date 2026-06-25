# Principle: Orchestrated Routing

> The orchestrator routes each task to an execution mode. It does not pre-assign
> roles.
> **Family:** Orchestration · **Placement Rule:** Q4 · **2026:** user-space

---

## Problem

Agent systems tend to one of two extremes. A single agent does everything —
context bloats, reasoning degrades. Or fixed roles are pre-assigned ("Agent A =
research, Agent B = code") — rigid, and wrong the moment a task crosses domains.
Both waste the attention budget: one by overload, the other by forcing the wrong
worker onto the task.

## Principle

Put a thin routing layer in front of execution. It reads the task and picks the
cheapest sufficient mode:

| Mode | When | How |
|------|------|-----|
| **Direct** | Simple, single-tool, fast | Handle in-context |
| **Iterative loop** | Complex, needs refinement | Goal loop with checkpoints |
| **Parallel delegate** | Independent sub-tasks | Spawn isolated workers |

The cheapest execution is direct; the most expensive is parallel delegation
(coordination overhead). The router's job is to find the minimum mode that works
— not to delegate by reflex.

## Theoretical grounding

- **Validated by** — Anthropic, *Building effective agents* (2024): distinguishes
  workflows from agents and recommends starting with the simplest pattern,
  adding complexity (and extra agents) only when it demonstrably helps.
- **Engineering judgment** — "minimum sufficient execution mode" is a design
  stance, applying the attention-budget thesis to orchestration: every extra
  worker is extra context to coordinate.

> *Note: earlier drafts of this repo cited fabricated sources ("Dochkina 2026",
> "Dynamite Delegation") for this principle. They have been removed. The
> principle stands on the Anthropic guidance above and on plain engineering
> reasoning — not on invented evidence.*

## Practical application (DIY)

- Make routing an explicit, first step — not an implicit habit of one big agent.
- Default to Direct. Escalate to a loop only when a task needs iteration; to
  parallel workers only when sub-tasks are genuinely independent.
- Treat the routing decision itself as a refinable skill — log mis-routes and
  tighten the heuristics.

## User-space vs. platform-space

**Yours.** Harnesses give you sub-agent primitives, but the routing *policy* —
when to stay direct, when to spawn — is your configuration. This is where a lot
of DIY hair-pulling lives, and where a clear rule pays off most.

## Anti-patterns

- **Always delegating** — overhead on tasks that didn't need it.
- **Never delegating** — single-agent context collapse on parallel work.
- **Static roles** — pre-assigned capabilities that fail across domains.

## Sources

| Source | Label | Link |
|--------|-------|------|
| Anthropic — Building effective agents (2024) | Validated by | https://www.anthropic.com/research/building-effective-agents |
