---
name: planning
description: >-
  Plan, implement, and validate ideas. Produces implementation plans, plan-mode documents, throwaway spikes, and file-based session tracking (task_plan.md, findings.md, progress.md). French triggers: plan, planifie, écris un plan, spike, expérimente, writing plan, implementation plan.
domain: agentic
category: agentic
importance_score: 8
generalization_score: 9
author: [USER_NAME] + [AGENT_NAME]
source:
  - skills-curated/built-in/plan/SKILL.md
  - skills-curated/built-in/writing-plans/SKILL.md
  - skills-curated/built-in/spike/SKILL.md
  - skills-curated/to-merge-or-classify/planning/writing-plans/SKILL.md
  - skills-curated/to-merge-or-classify/planning/writing-plans-custom/SKILL.md
  - skills-curated/to-merge-or-classify/planning/plan/SKILL.md
  - skills-curated/to-merge-or-classify/planning/planning-with-files/SKILL.md
merged_from:
  - plan
  - writing-plans
  - spike
  - planning-with-files
fork_date: 2026-05-27
consumes_official: false
suggested_loading: auto
core: false
keywords: [planning, implementation plan, spike, prototype, exploration, task plan, research, feasibility, proof-of-concept]
triggers:
  - "plan"
  - "planifie"
  - "écris un plan"
  - "spike"
  - "expérimente"
  - "writing plan"
  - "implementation plan"
---

# Planning Skill

## When to Load

Load this skill when the user wants to:
- **Plan mode** (pas d'exécution) — write a markdown plan without touching code. French triggers: "plan", "planifie", "écris un plan".
- **Implementation plan** — break down a multi-step feature into bite-sized tasks with exact file paths, complete code, and test commands. French triggers: "writing plan", "implementation plan".
- **Spike / throwaway experiment** — validate an idea before committing to a real build. French triggers: "spike", "expérimente".
- **File-based planning** — use task_plan.md, findings.md, and progress.md for session tracking, Manus-style context engineering.
- Combine these: e.g., spike first, then plan the implementation.

## Overview

This is a unified planning skill that merges three complementary workflows plus a file-based session tracker:

1. **Plan mode** — write-only planning. Inspect the repo, produce a markdown plan at `.hermes/plans/`. No code, no mutations.
2. **Implementation plans** — document every file, every line of code, every test command. Bite-sized tasks (2-5 min each). TDD. DRY. YAGNI. Frequent commits.
3. **Spikes** — disposable experiments to answer feasibility questions. Decompose, research, build, verdict. Standard and comparison (A/B) modes.
4. **File-based session tracking** — three Markdown files (`task_plan.md`, `findings.md`, `progress.md`) act as external memory. Plan attestation via SHA-256 prevents injection.

All four modes share a core philosophy: **a good plan makes implementation obvious. If someone has to guess, the plan is incomplete.**

## Workflow

### A. Plan Mode (no execution)

1. The user says "plan" or equivalent French trigger.
2. Inspect the repo or context with read-only tools.
3. Write a concrete, actionable plan to `.hermes/plans/YYYY-MM-DD_HHMMSS-<slug>.md`.
4. Include: goal, context/assumptions, proposed approach, step-by-step plan, files likely to change, tests/validation, risks/tradeoffs/open questions.
5. If unclear, ask one clarifying question — do not guess.
6. Reply with what you planned and the saved path.

### B. Implementation Plan

1. Understand requirements — feature requirements, acceptance criteria, constraints.
2. Explore the codebase — search files, read key files, check existing tests.
3. Design the approach — architecture pattern, file organization, dependencies, testing strategy.
4. Write bite-sized tasks (2-5 min each), ordered by dependency:
   - Setup/infrastructure
   - Core functionality (TDD for each task)
   - Edge cases
   - Integration
   - Cleanup/documentation
5. Each task must include:
   - **Exact file paths** (e.g. `src/models/user.py:45-67`) — never "the config file"
   - **Complete code examples** — copy-pasteable, never "add validation"
   - **Exact commands with expected output** — every test step has the run command AND expected outcome (PASS/FAIL)
6. Save the plan to `docs/plans/YYYY-MM-DD-feature-name.md` or propose execution via `subagent-driven-development`.

### C. Spike (throwaway experiment)

1. **Decompose** — break the idea into 2–5 independent feasibility questions. Frame as Given/When/Then.
2. **Order by risk** — the spike most likely to kill the idea runs first.
3. **Research** — per spike: brief it, surface competing approaches, pick one.
4. **Build** — one directory per spike under `spikes/NNN-descriptive-name/`. Bias toward something the user can interact with (CLI, HTML page, web server, unit test).
5. **Verdict** — VALIDATED / PARTIAL / INVALIDATED with "what worked", "what didn't", "surprises", "recommendation".
6. **Comparison spikes** (same question, different approaches): build back-to-back, then head-to-head table.
7. Keep code throwaway — never clean up a spike for production.

### D. File-Based Session Tracking

1. Create `task_plan.md`, `findings.md`, `progress.md` (use `init-session.sh` or `init-session.ps1`).
2. `task_plan.md` — goal, phases with checkboxes, key questions, decisions made, errors encountered.
3. `findings.md` — all discoveries, requirements, research, technical decisions.
4. `progress.md` — chronological session log with test results and error tracking.
5. Re-read `task_plan.md` before major decisions (attention manipulation — Manus Principle 4).
6. Optionally attest the plan with `attest-plan.sh` / `attest-plan.ps1` — hooks block injection if the file is tampered.
7. For multi-session work, use `.planning/<date>-<slug>/` directories and `.active_plan` pointer.

### E. Hybrid Workflow (all four combined)

1. User says "spike" → run spike workflow (C).
2. Spike returns VALIDATED → write implementation plan (B) with TDD steps.
3. User says "plan exécute" → save plan to `.hermes/plans/` (A) and optionally track via file-based session (D).
4. Execute via `subagent-driven-development`: fresh subagent per task, two-stage review (spec compliance then code quality).

## Tools & Commands

### Scripts (in `scripts/`)
| Script | Purpose |
|--------|---------|
| `init-session.sh` / `.ps1` | Bootstrap task_plan.md, findings.md, progress.md |
| `resolve-plan-dir.sh` / `.ps1` | Resolve active plan directory (env var → .active_plan → newest) |
| `set-active-plan.sh` / `.ps1` | Pin/unpin active plan |
| `attest-plan.sh` / `.ps1` | Lock plan file with SHA-256 to prevent injection |
| `check-complete.sh` / `.ps1` | Report phase completion status |
| `check-continue.sh` | Verify IDE integration files |
| `session-catchup.py` | Recover unsynced context after session restart (/clear) |
| `sync-ide-folders.py` | Sync shared assets to IDE-specific folders |

### Templates (in `templates/`)
| Template | Purpose |
|----------|---------|
| `task_plan.md` | Default task plan template |
| `findings.md` | Default findings template |
| `progress.md` | Default progress log template |
| `analytics_task_plan.md` | Data analytics variant |
| `analytics_findings.md` | Data analytics findings variant |

### References
- `reference.md` — Manus Context Engineering Principles (6 principles, 3 strategies, agent loop)
- `examples.md` — worked examples for research, bug fixes, feature development, error recovery

### Save locations
- Plan mode: `.hermes/plans/YYYY-MM-DD_HHMMSS-<slug>.md`
- Implementation plans: `docs/plans/YYYY-MM-DD-feature-name.md`
- Spikes: `spikes/NNN-descriptive-name/` (or `.planning/spikes/` for GSD users)
- File-based sessions: `task_plan.md`, `findings.md`, `progress.md` at project root or `.planning/<date>-<slug>/`

## Pitfalls

1. **Vague tasks in implementation plans.** Never write "Add validation" — show the exact code and the exact test.
2. **Giant tasks.** If a task takes more than 5 minutes or has more than 8 steps, split it. Bite-sized is non-negotiable.
3. **Missing file paths.** Every file reference must be an exact path from the repo root. Never "the config file" — always `src/config/settings.py:45-67`.
4. **Missing test commands.** Every test step needs the exact run command AND expected outcome (PASS/FAIL/ERROR).
5. **Spike without verdict.** Every spike must close with a verdict (VALIDATED / PARTIAL / INVALIDATED) and a recommendation. If there's no verdict, it wasn't a spike — it was just coding.
6. **Planning before understanding.** If the codebase is unfamiliar, explore it first. Use `codebase-inspection` skill before writing a plan.
7. **Over-planning simple changes.** Not every fix needs a plan — one-file bugs, typos, config changes: just fix them.
8. **Treating spikes as research.** Spikes are BUILD experiments, not reading. If the answer is knowable from docs, don't build — just research.
9. **Forgetting to update planning files.** After every phase, update `task_plan.md` status. Log ALL errors. The files are your persistent memory.
10. **Ignoring plan attestation.** If `task_plan.md` is attested and modified, hooks block injection. Re-run `attest-plan.sh` after intentional edits.

## Verification Checklist

- [ ] Plan mode: plan saved to `.hermes/plans/` with timestamped filename
- [ ] Implementation plan: every task has exact file paths, complete code, exact commands with expected output
- [ ] Spike: at least one directory under `spikes/` with README.md + code + verdict
- [ ] Spike verdict: VALIDATED | PARTIAL | INVALIDATED with recommendation
- [ ] File-based session: task_plan.md, findings.md, progress.md present and up-to-date
- [ ] Plan attestation active (optional but recommended for long-running tasks)
- [ ] All errors logged in the appropriate file
- [ ] Only one tool call per turn (Manus Principle — single-action execution)
- [ ] Failed actions were mutated, not silently retried

## References

- `templates/task_plan.md` — standard task plan template
- `templates/findings.md` — findings and decisions template
- `templates/progress.md` — progress log template
- `templates/analytics_task_plan.md` — analytics variant template
- `templates/analytics_findings.md` — analytics findings template
- `scripts/init-session.sh` — bootstrap planning files (shell)
- `scripts/init-session.ps1` — bootstrap planning files (PowerShell)
- `scripts/resolve-plan-dir.sh` — resolve active plan directory
- `scripts/resolve-plan-dir.ps1` — PowerShell mirror
- `scripts/set-active-plan.sh` — pin active plan
- `scripts/set-active-plan.ps1` — PowerShell mirror
- `scripts/attest-plan.sh` — lock plan with SHA-256
- `scripts/attest-plan.ps1` — PowerShell mirror
- `scripts/check-complete.sh` — report phase completion
- `scripts/check-complete.ps1` — PowerShell mirror
- `scripts/check-continue.sh` — verify IDE integration
- `scripts/session-catchup.py` — recover context after session reset
- `scripts/sync-ide-folders.py` — sync assets to IDE folders
- `reference.md` — Manus context engineering principles
- `examples.md` — worked examples

## Merged Sources

This skill merges 7 sources:

1. **skills-curated/built-in/plan/SKILL.md (plan)** — Plan mode: write-only, `.hermes/plans/` layout, no execution guarantees. Kept: core behavior, save location, interaction style.
2. **skills-curated/built-in/writing-plans/SKILL.md (writing-plans)** — Implementation plans with bite-sized tasks, TDD cycle, exact paths. Kept: task structure, writing process, principles (DRY/YAGNI/TDD), execution handoff.
3. **skills-curated/built-in/spike/SKILL.md (spike)** — Throwaway experiments with decompose→research→build→verdict loop. Kept: spike types, comparison mode, frontier mode, verdict format.
4. **skills-curated/to-merge-or-classify/planning/writing-plans/SKILL.md** — Duplicate of (2). Discarded as identical.
5. **skills-curated/to-merge-or-classify/planning/writing-plans-custom/SKILL.md** — Custom version with dynamic delegation pattern, commit conventions, project context (AGENTS.md), anti-patterns. Kept: delegation pattern, anti-patterns section.
6. **skills-curated/to-merge-or-classify/planning/plan/SKILL.md** — Duplicate of (1). Discarded as identical.
7. **skills-curated/to-merge-or-classify/planning/planning-with-files/SKILL.md** — File-based session tracking with task_plan.md/findings.md/progress.md, hooks, attestation. Kept: file templates, scripts, reference material, examples.
