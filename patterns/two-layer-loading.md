# Pattern: Two-Layer Loading

> Core skills are always loaded. Specialist skills load on trigger. Nothing else.

---

## Problem

Loading all skills eagerly wastes context. Not loading enough means the agent lacks necessary knowledge. The optimal point between these extremes is non-obvious.

## Solution

Two layers:

| Layer | Loading | Purpose | Size Target |
|-------|---------|---------|-------------|
| **Core** | Always injected at session start | Identity, safety, universal reflexes | ~15K tokens total |
| **Specialist** | Dynamically loaded via trigger matching | Domain procedures, deep analysis | Variable, loaded on demand |

The core layer is the minimum viable behavior set — the behaviors that fire in 80-100% of sessions regardless of task domain. Specialist skills fire in 5-30% of sessions.

## Trigger Mechanism

Specialist skills are discovered through **intent-derived keywords** in their YAML frontmatter:
- Name matching (task mentions skill name)
- Description matching (task semantically matches description)
- Tag matching (task overlaps with skill tags)

The agent scans available skills, detects matches, and loads relevant ones via `skill_view()`. This is mechanical, not need-based — research shows agents poorly judge whether a skill is truly necessary (SRA-Bench, 2026).

## Validation

- **SRA-Bench (Su et al., 2026):** 2-layer loading improves performance by 12-23 points over no-skill baseline. 8 distractor skills drop accuracy from 60% to 45% — this fragility does not disappear with larger models.
- **Stigmem (Stanford, 2026):** Compact boot stub + recall on demand = 50-87% token reduction, 0 regressions. Intent-derived keywords beat content-derived keywords for retrieval triggers.
- **Anthropic Agent Skills Architecture (2026):** 3 levels — system prompt (identity), skills (retrievable knowledge), context (per-session data). This pattern implements levels 1 and 2.

## Sizing Guidelines

| Component | Target | Scientific Basis |
|-----------|--------|-----------------|
| Identity layer | ≤1200 chars | Lost in the Middle + Persona Scaling Law |
| Core skills (each) | ≤5K tokens | SRA-Bench — beyond 5K, attention degrades |
| Core skills (total) | ≤15K tokens | Token Budget — static anchors should be 10-15% of window |
| Specialist skills | No hard limit | Loaded on demand, context available |

## Anti-Patterns

- **Everything in core:** Core skill bloat — putting specialist procedures in always-loaded skills
- **Too many core skills:** 5+ core skills = each degrades in attention. 3 is optimal.
- **No triggers:** Specialist skills without trigger keywords won't be discovered
- **Over-matching:** Too-broad triggers cause irrelevant skills to load

---

## Sources

| Source | Relevance |
|--------|-----------|
| Su et al. (2026) — SRA-Bench | 2-layer validated, distractor fragility |
| Stanford/Eidetic Labs (2026) — Stigmem | Lazy > preloading, intent keywords |
| Anthropic (2026) | 3-level architecture model |
| Tian Pan (2026) | Token budget allocation |
