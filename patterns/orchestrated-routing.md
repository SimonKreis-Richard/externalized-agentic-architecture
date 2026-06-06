# Pattern: Orchestrated Routing

> The orchestrator routes tasks to execution modes. It does not pre-assign roles.

---

## Problem

Agent systems often adopt one of two extremes:
1. **Single-agent:** One agent does everything — context bloats, reasoning degrades
2. **Static multi-agent:** Pre-assigned roles ("Agent A = research, Agent B = code") — rigid, fails on cross-domain tasks

Both waste resources. The single-agent burns context on procedures. The multi-agent system forces wrong agents to attempt tasks outside their capability.

## Solution

A routing layer that analyzes each task and selects the optimal execution mode:

| Mode | When | How |
|------|------|-----|
| **Direct** | Simple, <2 min, single-tool | Agent handles in-context |
| **Iterative Loop** | Complex, needs refinement | Goal-directed loop with checkpoints |
| **Parallel Delegate** | Independent subtasks | Spawn isolated workers |

The routing decision itself is a learnable skill — the orchestrator gets better at routing through experience.

## Key Insight: Routing > Delegation

"Dynamite delegation" (Do, 2024) showed that dynamic delegation — where the orchestrator analyzes each task and composes the optimal worker — outperforms static role assignment. But the insight extends further: **not every task needs delegation.**

The cheapest execution is direct. The most expensive is parallel delegation (multiple workers, coordination overhead). The orchestrator's job is to find the minimum sufficient execution mode.

## Validation

- **Dochkina (2026):** Dynamic delegation > fixed roles in task completion and resource efficiency
- **Dynamite Delegation (Do, 2024):** 38.7% improvement on GAIA benchmark with dynamic routing
- **SRA-Bench (2026):** Pre-loading capabilities wastes context — route to what's needed, not what exists

## Anti-Patterns

- **Always delegating:** Simple tasks don't need workers. Delegation has overhead.
- **Never delegating:** Complex parallel tasks suffer in single-agent context.
- **Static roles:** Pre-assigning capabilities creates rigidity.
- **Blind routing:** Routing without understanding task requirements leads to wrong mode selection.

---

## Sources

| Source | Relevance |
|--------|-----------|
| Dochkina (2026) | Dynamic delegation > fixed roles |
| Do (2024) — Dynamite Delegation | 38.7% improvement on GAIA |
| Su et al. (2026) — SRA-Bench | Pre-loading wastes context |
