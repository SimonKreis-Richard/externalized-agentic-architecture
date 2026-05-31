---
name: project-context
description: >-
  Load this skill when entering any new or unfamiliar project repository to create or update AGENTS.md,
  CLAUDE.md, or .cursorrules — the canonical project-level context files. Automatically triggers the
  SOUL reflex (« Nouveau projet → AGENTS.md »). Handles structural repo scan, deep read, synthesis,
  context budget enforcement, and maintenance. French trigger phrases: « contexte projet », « AGENTS.md »,
  « CLAUDE.md », « project context », « initialise le projet ».
domain: agentic
category: methodology
importance_score: 8
generalization_score: 8
author: '[USER_NAME] + [AGENT_NAME]'
source:
  - skills-curated/custom-core/project-context-methodology/SKILL.md
merged_from:
  - project-context-methodology
fork_date: 2026-05-27
consumes_official: false
suggested_loading: auto
core: false
keywords:
  - project context
  - AGENTS.md
  - CLAUDE.md
  - .cursorrules
  - onboarding
  - repo scan
  - context injection
  - context budget
triggers:
  - "contexte projet"
  - "AGENTS.md"
  - "CLAUDE.md"
  - "project context"
  - "initialise le projet"
  - "initialiser le projet"
  - "analyse le repo"
---

## When to Load

| Condition | Action |
|-----------|--------|
| **First time working in a repo** | Run the full onboarding protocol (Pass 1–3) and create AGENTS.md |
| **User says « initialise le projet » / « contexte projet »** | Immediately run the full protocol |
| **Major architectural change** (refactor, new service, DB change) | Update existing AGENTS.md |
| **New tool or dependency added** (framework, linter, test runner) | Add to Tech Stack / Commands sections |
| **Conventions changed** (commit style, branch strategy, testing approach) | Update Conventions section |
| **Repetitive explanation pattern** (explaining same thing to subagent twice) | Add to AGENTS.md |

AGENTS.md is the canonical name for Hermes. The system also checks `CLAUDE.md`, `.cursorrules`, and `.windsurfrules` in the project root — load this skill when any of those need creation or maintenance.

## Overview

An agent arriving in a new project has **zero context** — no knowledge of the codebase, conventions, dependencies, or architecture. Without a project context file, every session starts with a cold discovery phase, wasting tokens and risking hallucinated assumptions.

This skill defines the **onboarding protocol**: a three-pass method (structural scan → deep read → synthesis) that produces a compact AGENTS.md in under 2 minutes. It enforces **context budget discipline** — the file must stay under 200 lines / ~8 KB to avoid bloating the agent's limited context window. It also governs maintenance cadence, integration with Hermes workdir auto-injection, and the distinction between project context (AGENTS.md) vs. system-level context (0-hermes-single-source-of-truth/system-design.md).

Adapted for bilingual (FR/EN) environments.

## Workflow

### Pass 1: Structural Scan (fast, 30s)

Map the repo surface with minimal tool calls:

```bash
# Directory tree — top 3 levels, exclude noise
tree -L 3 -I 'node_modules|.git|__pycache__|.venv|.tox|dist|build|.next'

# Config files at root
fd -d 2 -e json -e yaml -e yml -e toml -e cfg -e ini . 2>/dev/null || \
  find . -maxdepth 2 \( -name '*.json' -o -name '*.yaml' -o -name '*.yml' -o -name '*.toml' -o -name '*.cfg' \) | grep -v node_modules

# Dependency manifest
cat package.json 2>/dev/null || cat pyproject.toml 2>/dev/null || cat Cargo.toml 2>/dev/null || cat composer.json 2>/dev/null || true
```

Also note the **language distribution**: run `fd -e py -e ts -e js -e rs -e go -e java | head -5` to identify the primary language.

### Pass 2: Deep Read (targeted, 1–2 min)

Read the critical files that reveal architecture and conventions:

1. **Entry point** — `main.py`, `index.ts`, `app.js`, `cmd/`, or `src/main.rs`
2. **README** — often contains setup, architecture notes, and key links
3. **Config files** — `.env.example`, `docker-compose.yml`, CI config, Makefile
4. **Test structure** — `tests/` or `__tests__/` directory layout reveals org patterns
5. **CI/CD config** — `.github/workflows/`, `.gitlab-ci.yml` — reveals test/lint commands

**Context budget check**: If any single file exceeds ~300 lines, read only its table of contents / section headers. Don't load entire large files into context during onboarding.

### Pass 3: Write AGENTS.md

Synthesize findings into the template below. Place in **project root**.

```markdown
# Project: [NAME]

## Overview
[1-2 sentences: what this project does, who it's for]

## Architecture
- [Key architectural decisions]
- [Directory structure with purpose of each folder]
- [Data flow: how data moves through the system]

## Tech Stack
- Language: [Python 3.12 / TypeScript 5.x / etc.]
- Framework: [FastAPI / React / Next.js / etc.]
- Database: [PostgreSQL / SQLite / Redis / etc.]
- Key dependencies: [list critical ones, not every package]

## Conventions
- **Code style:** [Black / Ruff / Prettier / ESLint]
- **Testing:** [pytest -n auto / Jest --coverage — include exact command]
- **Commits:** [Conventional commits / semantic-release / custom format]
- **Branching:** [main/dev / trunk-based / Git Flow]
- **PR process:** [squash merge / rebase merge / review requirements]

## Commands
```bash
# Install
npm install

# Test
pytest -n auto

# Build
npm run build

# Lint / Format
ruff check .
```

## Key Files
- `src/main.py` — Entry point
- `config/settings.yaml` — Configuration
- `docs/architecture.md` — Design decisions

## Constraints
- [Anything unusual: must work offline, no external APIs, specific Node/py version, platform-specific compatibility]

## External Dependencies
- [APIs, services, databases — with config locations and access info]
```

**Context budget discipline:**
- Target: **under 200 lines** (ideally 120–180)
- Target: **under 8 KB** raw file size
- If more detail is needed, link to docs/ files rather than inline
- Mark optional/meta sections (like "External Dependencies") with a comment so an agent can skip them when loading under tight context budgets

### Maintenance Cadence

**Update when:**
- Architecture changes (new service, refactored module, DB migration)
- New major dependency added (framework, database, critical library)
- Conventions change (new linter, test framework, commit style)
- Constraints change (new API requirement, Node/py version bump, platform-specific workaround added)

**Don't update when:**
- Minor dependency bumps (patch versions)
- Bug fixes that don't alter structure
- Adding features within existing architecture
- Simple refactors that keep the same patterns

**Rule of thumb:** If you find yourself explaining the same thing to a subagent twice, it belongs in AGENTS.md.

## Tools & Commands

| Tool / Command | Purpose |
|----------------|---------|
| `tree -L 3 -I 'node_modules\|.git\|__pycache__\|.venv'` | Fast structural scan of repo |
| `fd -d 2 -e json -e yaml -e yml -e toml` | Find config files fast |
| `cat package.json` / `cat pyproject.toml` | Read dependency manifest |
| `read_file` (Hermes tool) | Deep read entry points and README |
| `search_files` (Hermes tool) | Find specific patterns during scan |
| AGENTS.md template (above) | Standardized project context format |
| `ls -la AGENTS.md CLAUDE.md .cursorrules .windsurfrules 2>/dev/null` | Check existing context files in project root |

## Pitfalls

1. **Creating AGENTS.md without scanning the repo first.** You will hallucinate architecture, directory structure, and dependencies — producing a file that is actively harmful (it misleads future sessions). Always run Pass 1 and Pass 2 before writing.

2. **Copy-pasting the README.** READMEs are written for humans deploying or contributing to the project — they often omit architecture decisions, test commands, and constraints that an agent actually needs. Synthesize what the agent needs to be productive from turn 1.

3. **Letting AGENTS.md grow beyond 200 lines / 8 KB.** Every line of AGENTS.md competes for space in the context window against task instructions, tool outputs, and conversation history. Bloated context files get silently truncated or skimmed. Be ruthless with brevity. Link to detailed docs rather than inline them.

4. **Never updating AGENTS.md after creation.** Stale context is worse than no context — it causes confident wrong answers based on outdated architecture. Attach a review trigger to major architectural events (new service, DB change, framework upgrade).

5. **Storing secrets or credentials in AGENTS.md.** Context files are loaded into the LLM context window and may be logged, cached, or leaked through training data. Secrets go in `.env` files, never in AGENTS.md / CLAUDE.md / .cursorrules.

6. **Confusing AGENTS.md with 0-hermes-single-source-of-truth/system-design.md.** AGENTS.md is project-scoped, per-repo, and high-churn. Canonical decisions are system-scoped, cross-project, and low-churn. Put the right content in the right file.

7. **Putting task state or TODOs in AGENTS.md.** Task state is ephemeral and session-specific. AGENTS.md is durable context that outlives any single session. Use session_search or the Hermes todo system for task tracking.

## Verification Checklist

- [ ] AGENTS.md exists in the project root
- [ ] File is under 200 lines and under 8 KB (`wc -l && wc -c`)
- [ ] All sections present: Overview, Architecture, Tech Stack, Conventions, Commands, Key Files, Constraints
- [ ] Commands are tested — `pytest -n auto` actually runs in this project
- [ ] No secrets, passwords, or API keys in the file
- [ ] No placeholder text, no `[TODO]`, no empty sections
- [ ] Architecture description matches actual directory structure (re-verify with `tree`)
- [ ] Tech Stack versions match actual dependency manifest
- [ ] CI/CD commands (lint, test, build) match the CI config files
- [ ] Platform-specific notes included if applicable (e.g., path translation patterns for WSL)
- [ ] If CLAUDE.md / .cursorrules / .windsurfrules exist, they are consistent with AGENTS.md
- [ ] Context budget respected: optional/meta sections marked as skippable under tight window

## References

- **Template**: The AGENTS.md template is defined inline in this skill (Workflow → Pass 3 section)
- **Attached files**: None — this is a standalone methodology skill

## Merged Sources

| Source | What was kept | What was discarded |
|--------|---------------|-------------------|
| `skills-curated/custom-core/project-context-methodology/SKILL.md` | Full methodology: three-pass onboarding protocol, AGENTS.md template, maintenance rules, integration with Hermes system, anti-patterns | Redundant versioning info, duplicate section headers, user-specific notes absorbed into adaptation layer |

**Original skill name:** `project-context-methodology` (category: meta, core: true)
**This is a definitive merge:** one concept → one skill. All unique insights preserved. Adapted for bilingual FR/EN environments, WSL ecosystem, and context budget discipline.
