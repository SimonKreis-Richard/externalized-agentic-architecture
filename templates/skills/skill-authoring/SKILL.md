---
name: skill-authoring
description: "The unified lifecycle system for creating, validating, packaging, classifying, and discovering Hermes skills."
domain: hermes
importance_score: 9
generalization_score: 9
core: false
suggested_loading: auto
author: '[USER_NAME] + [AGENT_NAME]'
source:
  - skills-curated/anthropic/skill-creator/SKILL.md
  - skills-curated/built-in/hermes-agent-skill-authoring/SKILL.md
  - skills-curated/custom-core/skills-classifier/SKILL.md
  - skills-curated/to-merge-or-classify/skills-management/skill-creator/SKILL.md
  - skills-curated/to-merge-or-classify/skills-management/hermes-agent-skill-authoring/SKILL.md
  - skills-curated/to-merge-or-classify/skills-management/mcporter/SKILL.md
  - skills-curated/to-merge-or-classify/skills-management/hub-scout/SKILL.md
  - skills-curated/to-merge-or-classify/skills-management/skills-config-management/SKILL.md
  - skills-curated/to-merge-or-classify/skills-management/hermes-skills-architecture (merge avec anthropic creation skill)/SKILL.md
merged_from:
  - anthropic/skill-creator
  - hermes-agent-skill-authoring (built-in)
  - skills-classifier
  - skill-creator (skills-management)
  - hermes-agent-skill-authoring (skills-management)
  - mcporter
  - hub-scout
  - skills-config-management
  - hermes-skills-architecture
fork_date: 2026-05-27
keywords: skill, authoring, create, validate, package, classify, hub, scout, mcporter, eval, benchmark, architecture
triggers:
  - "crée un skill"
  - "nouveau skill"
  - "author a skill"
  - "classifie les skills"
  - "crée un nouveau skill"
  - "package un skill"
  - "valide un skill"
  - "optimise la description"
  - "évalue un skill"
  - "benchmark skill"
  - "skill authoring"
  - "skill creation"
  - "skill packaging"
  - "skill validation"
  - "skill classification"
  - "skill hub"
  - "skill scout"
  - "skill management"
  - "skill config"
  - "skill architecture"
  - "créer un skill"
  - "créer une compétence"
  - "modifier un skill"
  - "améliorer un skill"
  - "tester un skill"
  - "évaluer un skill"
  - "package skill"
  - "installer skill"
  - "découvrir skill"
  - "rechercher skill"
  - "architecture des skills"
  - "mcporter skill"
  - "MCP skill"
  - "skill hub"
  - "skill registry"
  - "classifier skill"
  - "grader skill"
  - "comparator skill"
  - "analyzer skill"
  - "eval set"
  - "benchmark"
  - "variance analysis"
keywords_english:
  - skill authoring
  - skill creation
  - skill validation
  - skill packaging
  - skill classification
  - skill hub
  - skill scout
  - skill management
  - skill config
  - skill architecture
  - skill evaluation
  - skill benchmark
  - MCP skills
  - eval set
  - description optimization
---

# Skill-Authoring

> Le système unifié pour créer, rédiger, valider, empaqueter, classifier et découvrir des skills Hermes. The unified lifecycle system for the entire Hermes skill lifecycle.

This is the **definitive** skill for everything related to Hermes skill authoring: from creation to validation, packaging, classification, hub discovery, evaluation, and architectural understanding. It merges nine source skills into one comprehensive reference.

---

## Table des matières / Table of Contents

1. [Architecture des Skills / Skill Architecture](#architecture-des-skills--skill-architecture)
2. [Création d'un Skill / Creating a Skill](#création-dun-skill--creating-a-skill)
3. [Rédaction In-Repo / Authoring In-Repo Skills](#rédaction-in-repo--authoring-in-repo-skills)
4. [Validation des Skills / Skill Validation](#validation-des-skills--skill-validation)
5. [Classification des Skills / Skill Classification](#classification-des-skills--skill-classification)
6. [Empaquetage / Packaging](#empaquetage--packaging)
7. [Hub Discovery / Scout](#hub-discovery--scout)
8. [Évaluation et Benchmark / Eval & Benchmark](#évaluation-et-benchmark--eval--benchmark)
9. [Optimisation de Description / Description Optimization](#optimisation-de-description--description-optimization)
10. [Gestion de Configuration / Config Management](#gestion-de-configuration--config-management)
11. [Intégration MCP / MCP Integration](#intégration-mcp--mcp-integration)
12. [Agents d'Aide / Helper Agents](#agents-daide--helper-agents)
13. [Références / References](#références--references)
14. [Pièges Communs / Common Pitfalls](#pièges-communs--common-pitfalls)
15. [Checklist de Vérification / Verification Checklist](#checklist-de-vérification--verification-checklist)

---

## 1. Architecture des Skills / Skill Architecture

Comprendre les DEUX systèmes de skills d'Hermes, pourquoi `hermes skills install` ne trouve pas tout, et comment maintenir sa skill library.

### Les Deux Sources de Skills

Hermes charge les skills depuis DEUX mécanismes complètement distincts :

#### 1. Hub (distant)
- **Commande :** `hermes skills install <nom>`
- **Sources :** lobehub, official (Nous Research), GitHub (skills.sh), clawhub
- **Résolution :** cherche dans un index distant par nom exact
- **Stockage :** copié dans `{SKILLS_DIR}/<nom>/`
- **Source dans le listing :** `lobehub`, `official`, `github`, `clawhub`, `skills.sh`

#### 2. Repo local (natives / builtin)
- **Emplacement :** `{HERMES_AGENT_PATH}/skills/<category>/<nom>/` (editable install)
- **Source :** livrées avec le package Hermes (`pip install -e`)
- **Résolution :** scan local, pas de hub — Hermes les découvre via le filesystem
- **Source dans le listing :** `builtin`

> **Ils ne communiquent pas entre eux.** `hermes skills install arxiv` échoue car arxiv n'est pas dans le hub — il est dans le repo.

### Où les skills sont stockées

```
{HOME_DIR}.hermes/
├── skills/                           # Dossier par défaut
│   ├── arxiv/                        # Natives (repérées automatiquement)
│   ├── obsidian/
│   ├── plan/
│   └── 0-custom-skills/              # Customs (via external_dirs)
│       ├── autonomie-maintenance/
│       ├── skill-design/
│       └── ...
├── hermes-agent/skills/              # Repo source (editable install)
│   ├── research/arxiv/
│   ├── note-taking/obsidian/
│   └── ...
└── config.yaml                        # external_dirs pointe vers 0-custom-skills/
```

#### Mécanisme external_dirs
- Config: `skills.external_dirs` dans `{CONFIG_PATH}`
- Hermes scanne CE dossier EN PLUS de `{SKILLS_DIR}/`
- Les skills dans `external_dirs` apparaissent avec leur dossier parent comme catégorie

### Architecture voulue ([USER_NAME] + [AGENT_NAME])

```
{SKILLS_DIR}/
├── <toutes les natives ici>          # 54+ skills à la racine
└── 0-custom-skills/                  # Un dossier unique pour nos 33+ customs
    ├── autonomie-maintenance/
    ├── ... toutes nos créations ici
    └── skill-design/
```

- **Jamais** de customs dispersées dans des sous-catégories à la racine
- **Jamais** de natives dans `0-custom-skills/`
- Créer une custom : passer `category: 0-custom-skills` à `skill_manage(action='create')` ou déplacer manuellement après création

---

## 2. Création d'un Skill / Creating a Skill

There are two places a SKILL.md can live:

1. **User-local:** `{SKILLS_DIR}/<maybe-category>/<name>/SKILL.md` — personal, not shared. Created via `skill_manage(action='create')`.
2. **In-repo:** `skills/<category>/<name>/SKILL.md` — committed, shipped with the package. Use `write_file` + `git add`. `skill_manage(action='create')` does NOT target this tree.

### Required Frontmatter

Source of truth: `tools/skill_manager_tool.py::_validate_frontmatter`. Hard requirements:

- Starts with `---` as the first bytes (no leading blank line).
- Closes with `\n---\n` before the body.
- Parses as a YAML mapping.
- `name` field present.
- `description` field present, ≤ **1024 chars** (`MAX_DESCRIPTION_LENGTH`).
- Non-empty body after the closing `---`.

Peer-matched shape used by every skill under `skills/`:

```yaml
---
name: my-skill-name               # lowercase, hyphens, ≤64 chars (MAX_NAME_LENGTH)
description: Use when <trigger>. <one-line behavior>.
version: 1.0.0
author: Your Name
license: MIT
metadata:
  hermes:
    tags: [short, descriptive, tags]
    related_skills: [other-skill, another-skill]
---
```

`version` / `author` / `license` / `metadata` are NOT enforced by the validator, but every peer has them — omit and your skill sticks out.

### Size Limits

- Description: ≤ 1024 chars (enforced).
- Full SKILL.md: ≤ 100,000 chars (enforced as `MAX_SKILL_CONTENT_CHARS`, ~36k tokens).
- Peer skills in `software-development/` sit at **8-14k chars**. Aim for that range. If you're pushing past 20k, split into `references/*.md` and reference them from SKILL.md.

### Peer-Matched Structure

Every skill follows roughly:

```
# <Title>

## Overview
One or two paragraphs: what and why.

## When to Use
- Bulleted triggers
- "Don't use for:" counter-triggers

## <Topic sections specific to the skill>
- Quick-reference tables are common
- Code blocks with exact commands
- Hermes-specific recipes

## Common Pitfalls
Numbered list of mistakes and their fixes.

## Verification Checklist
- [ ] Checkbox list of post-action verifications

## One-Shot Recipes (optional)
Named scenarios → concrete command sequences.
```

Not every section is mandatory, but `Overview` + `When to Use` + actionable body + pitfalls are the minimum for the skill to feel like a peer.

### Directory Placement

```
skills/<category>/<skill-name>/SKILL.md
```

Categories currently in repo: `autonomous-ai-agents`, `creative`, `data-science`, `devops`, `dogfood`, `email`, `gaming`, `github`, `leisure`, `mcp`, `media`, `mlops/*`, `note-taking`, `productivity`, `red-teaming`, `research`, `smart-home`, `social-media`, `software-development`.

Pick the closest existing category. Don't invent new top-level categories casually.

### Workflow

1. **Survey peers** in the target category: `ls skills/<category>/`
2. Read 2-3 peer SKILL.md files to match tone and structure.
3. **Check validator constraints** in `tools/skill_manager_tool.py` if unsure.
4. **Draft** with `write_file` to `skills/<category>/<name>/SKILL.md`.
5. **Validate locally** (see [Validation section](#validation-des-skills--skill-validation)).
6. **Git add + commit** on the active branch.
7. **Note:** the CURRENT session's skill loader is cached — `skill_view` / `skills_list` will not see the new skill until a new session.

### Cross-Referencing Other Skills

`metadata.hermes.related_skills` unions both trees (`skills/` in-repo and `{SKILLS_DIR}/`) at load time. You CAN reference a user-local skill from an in-repo skill, but it won't resolve for other users who clone the repo fresh. Prefer referencing only in-repo skills from in-repo skills.

### Editing Existing Skills

- **Small fix:** `skill_manage(action='patch', name=..., old_string=..., new_string=...)` works fine.
- **Major rewrite:** `write_file` the whole SKILL.md.
- **Adding supporting files:** `write_file` to `references/<file>.md`, `templates/<file>`, or `scripts/<file>`.

---

## 3. Rédaction In-Repo / Authoring In-Repo Skills

When you need to commit a skill to the Hermes Agent repository itself (shipped with the package), follow these conventions.

### When to Use

- User asks you to add a skill "in this branch / repo / commit"
- You're committing a reusable workflow that should ship with hermes-agent
- You're editing an existing skill under the repo's `skills/` directory

### Required Frontmatter (In-Repo Specifics)

Same base requirements as above, plus:

| Field | Rule | Enforced? |
|-------|------|-----------|
| `name` | lowercase + hyphens + digits, ≤ 64 chars | Yes |
| `description` | present, ≤ 1024 chars | Yes |
| `version` | semantic (e.g. 1.0.0) | No, but expected |
| `author` | e.g. "Hermes Agent" | No, but expected |
| `license` | e.g. "MIT" | No, but expected |
| `metadata.hermes.tags` | list of descriptive tags | No, but expected |
| `metadata.hermes.related_skills` | list of peer skill names | No, but expected |

### In-Repo Editing Patterns

- **Small fix (typo, added pitfall, tightened trigger):** `skill_manage(action='patch')` works on in-repo skills.
- **Major rewrite:** `write_file` the whole SKILL.md.
- **Adding supporting files:** `write_file` to `skills/<category>/<name>/references/<file>.md`, `templates/<file>`, or `scripts/<file>`.
- **Always commit** the edit — in-repo skills are source, not runtime state.

### Local Validation (One-Shot)

```python
import yaml, re, pathlib
content = pathlib.Path("skills/<category>/<name>/SKILL.md").read_text()
assert content.startswith("---")
m = re.search(r'\n---\s*\n', content[3:])
fm = yaml.safe_load(content[3:m.start()+3])
assert "name" in fm and "description" in fm
assert len(fm["description"]) <= 1024
assert len(content) <= 100_000
```

---

## 4. Validation des Skills / Skill Validation

Use the `quick_validate.py` script to validate a skill's SKILL.md against formal requirements.

### Usage

```bash
python scripts/quick_validate.py <skill_directory>
```

Exit code 0 = valid, 1 = invalid.

### Validation Checks

The validator (`scripts/quick_validate.py`) checks:

1. **SKILL.md exists** in the specified directory.
2. **YAML frontmatter is present** (starts with `---`).
3. **Frontmatter is valid YAML** and parses as a dictionary.
4. **Allowed properties only:** `name`, `description`, `license`, `allowed-tools`, `metadata`, `compatibility`.
5. **`name` is required**, must be a string, kebab-case (lowercase letters, digits, hyphens), ≤ 64 chars, no leading/trailing/consecutive hyphens.
6. **`description` is required**, must be a string, ≤ 1024 chars, no angle brackets.
7. **`compatibility`** (optional) must be a string ≤ 500 chars.

### Package Validation

Before packaging, `package_skill.py` automatically runs validation. If the skill fails, packaging is aborted with an error message.

---

## 5. Classification des Skills / Skill Classification

Use the classification framework to categorize skills by their type, domain, and characteristics.

### Classification Dimensions

Skills can be classified along several axes:

1. **Type:** `system` (core infrastructure), `productivity` (user-facing workflow), `research` (information gathering), `creative` (content generation), `data-science` (analysis), `devops` (operations).
2. **Domain:** `hermes`, `anthropic`, `custom-core`, `software-development`, `research`, `general`, etc.
3. **Core vs Optional:** Core skills should load always (`suggested_loading: always`), optional skills load on demand.
4. **Importance Score:** 1-10 scale (9 = critical system skill, 1 = niche utility).
5. **Generalization Score:** 1-10 scale (9 = broad applicability, 1 = highly specific).

### Scoring Guidelines

| Score | Importance | Generalization |
|-------|-----------|---------------|
| 9-10 | System-critical, must load | Universal applicability |
| 7-8 | Foundational, often used | Wide applicability |
| 5-6 | Useful but not essential | Moderate applicability |
| 3-4 | Niche, occasional use | Narrow applicability |
| 1-2 | Rarely needed | Highly specific |

### Classifier Agent

A dedicated classifier agent (`agents/analyzer.md`) can analyze a skill and produce a structured classification with:

- Identified triggers and counter-triggers
- Suggested importance and generalization scores
- Categorization into the skill taxonomy
- Recommendations for improvements

---

## 6. Empaquetage / Packaging

The `package_skill.py` script creates a distributable `.skill` file (zip format) from a skill folder.

### Usage

```bash
python scripts/package_skill.py <path/to/skill-folder> [output-directory]
```

Example:
```bash
python scripts/package_skill.py skills/public/my-skill
python scripts/package_skill.py skills/public/my-skill ./dist
```

### What Gets Packaged

- All files under the skill directory **except:**
  - `__pycache__` and `node_modules` directories (any depth)
  - `evals/` directory at the skill root
  - `*.pyc` files
  - `.DS_Store` files

### Validation Before Packaging

The packager automatically runs `validate_skill()` before creating the archive. If validation fails, packaging is aborted with a descriptive error message.

### Output

Produces `<skill-name>.skill` in the specified output directory (or current directory). This is a standard zip file that can be distributed and installed.

---

## 7. Hub Discovery / Scout

Proactively search the Hermes hub and external web repositories for skills relevant to your domains.

### Search Approach

1. **Hermes Hub:** Search via `hermes skills search <query>` or check the hub index.
2. **External Registries:** Check GitHub, skills.sh, lobehub, clawhub for published skills.
3. **Domain Targeting:** Focus on relevant domains (productivity, data analysis, system architecture, communications, research, creative, devops).
4. **Ranking:** Deliver a ranked candidate list with relevance scores, sources, and install commands.

### Installation Pitfalls

See `references/installation-pitfalls.md` for a comprehensive guide on common installation issues and how to resolve them — covering hub connectivity, registry variations, naming conflicts, and post-install verification.

### Hub Sources

| Source | Command | Scope |
|--------|---------|-------|
| Lobehub | `hermes skills install <name>` | Public skill registry |
| Official (Nous) | `hermes skills install <name>` | First-party skills |
| GitHub | `hermes skills install <name>` | Community skills on GitHub |
| Clawhub | `hermes skills install <name>` | Community registry |
| skills.sh | `hermes skills install <name>` | Aggregated registry |

---

## 8. Évaluation et Benchmark / Eval & Benchmark

Run evaluations to test skill performance, measure triggering accuracy, and benchmark with variance analysis.

### Evaluation Framework (run_eval.py)

The `run_eval.py` script tests a skill's description against a set of eval queries to measure how well it decides when to trigger.

```bash
python scripts/run_eval.py --skill-path <skill-dir> --eval-set <eval-set.json>
```

**Eval Set Format** (JSON):
```json
[
  {"query": "crée un nouveau skill", "should_trigger": true},
  {"query": "quelle heure est-il", "should_trigger": false}
]
```

This runs multiple trials of each query through the LLM and measures how often the skill triggers correctly.

### Optimization Loop (run_loop.py)

Iteratively improve a skill's description using a feedback loop:

```bash
python scripts/run_loop.py <skill-path> <eval-set.json> [--model <MODEL_NAME>]
```

The loop:
1. Evaluates the current description
2. Identifies failures (missed triggers + false triggers)
3. Calls the LLM to generate improved descriptions
4. Re-evaluates and picks the best performer
5. Repeats until convergence or max iterations

### Benchmark Aggregation (aggregate_benchmark.py)

Run multiple evaluation trials and aggregate results for variance analysis:

```bash
python scripts/aggregate_benchmark.py --results <results.json> --output <report.json>
```

Features:
- Statistical variance analysis across runs
- Per-query pass/fail rates
- Aggregate accuracy metrics
- Confidence intervals

### Report Generation (generate_report.py)

Generate visual HTML reports from loop output:

```bash
python scripts/generate_report.py <loop-output.json> -o report.html [--skill-name "My Skill"]
```

Features:
- Iteration-by-iteration comparison table
- Green checkmarks / red crosses per query
- Train vs test query separation
- Best-iteration highlighting
- Auto-refresh for live monitoring

### Eval Set Review (eval_review.html)

An interactive HTML tool (`assets/eval_review.html`) for reviewing and editing eval query sets. Features:
- Browse, add, edit, and delete queries
- Toggle "should trigger" per query
- Sort by trigger category
- Export the modified set as JSON

---

## 9. Optimisation de Description / Description Optimization

Improve a skill's description based on eval results using `improve_description.py`.

### How It Works

The description optimizer (`scripts/improve_description.py`) calls the LLM to generate improved descriptions by:

1. Analyzing **failed triggers** — queries that should have triggered but didn't
2. Analyzing **false triggers** — queries that triggered but shouldn't have
3. Generalizing from specific failures to broader intent categories (avoiding overfitting)
4. Generating a new description ≤ 1024 characters

### Usage

```bash
python scripts/improve_description.py \
  --eval-results <eval-results.json> \
  --skill-path <skill-dir> \
  --model <MODEL_NAME> \
  --history <previous-attempts.json> \
  --verbose
```

### Key Principles

- **Imperative phrasing:** "Use this skill for" rather than "this skill does"
- **Intent-focused:** Describe what the user is trying to achieve, not implementation details
- **Distinctive:** The description competes with other skills for attention
- **Concise:** 100-200 words max, hard limit of 1024 characters
- **Generalize:** Don't overfit to specific test queries; generalize to broader categories

### Safety Net

If the generated description exceeds 1024 characters, the system automatically makes a second call asking for a shorter rewrite.

---

## 10. Gestion de Configuration / Config Management

Control which skills are active via the `skills.disabled` config directive.

### How It Works

```yaml
skills:
  disabled:
  - brainstorming
  - defuddle
  - nano-pdf
  - powerpoint
```

Source: `agent/skill_utils.py:get_disabled_skill_names()` reads this list and filters skills by **exact name match**. No wildcards, no globs, no patterns.

- A skill **absent** from the disabled list = enabled
- A skill **in** the disabled list = never loaded
- `hermes skills install` does NOT add to disabled → auto-enabled

### Whitelist Pattern (Recommended)

When you have a curated list and want everything else silent:

1. **Identify ALL currently-installed bundled/hub skills** you DON'T want
2. **Add them to the disabled list** → everything is now muted
3. **Install desired skills** via `hermes skills install` → they auto-enable
4. **Local custom skills** stay enabled as long as absent from the disabled list

### Blacklist Pattern

The inverse: everything enabled except specific entries. Default Hermes behavior. Less safe for curated setups — new skills from updates or hub sync auto-enable.

### Per-Platform Disabling

Config supports platform-specific disabling via `skills.platform_disabled`. Format is identical to the global disabled list.

### Verification

```bash
hermes skills list | grep disabled
```

---

## 11. Intégration MCP / MCP Integration

Use `mcporter` to discover, call, and manage [MCP (Model Context Protocol)](https://modelcontextprotocol.io/) servers and tools directly from the terminal — useful for building skills that integrate with external tools and MCP servers.

### Prerequisites

Requires Node.js:
```bash
# No install needed (runs via npx)
npx mcporter list

# Or install globally
npm install -g mcporter
```

### Quick Start

```bash
# List MCP servers already configured on this machine
mcporter list

# List tools for a specific server with schema details
mcporter list <server> --schema

# Call a tool
mcporter call <server.tool> key=value
```

### Discovering MCP Servers

mcporter auto-discovers servers configured by other MCP clients (Claude Desktop, Cursor, etc.) on the machine:

```bash
# Connect to any MCP server by URL (no config needed)
mcporter list --http-url https://some-mcp-server.com --name my_server

# Or run a stdio server on the fly
mcporter list --stdio "npx -y @modelcontextprotocol/server-filesystem" --name fs
```

### Calling Tools

```bash
# Key=value syntax
mcporter call linear.list_issues team=ENG limit:5

# Function syntax
mcporter call "linear.create_issue(title: \"Bug fix needed\")"

# Ad-hoc HTTP server (no config needed)
mcporter call https://api.example.com/mcp.fetch url=https://example.com

# Machine-readable output (recommended for Hermes)
mcporter call <server.tool> key=value --output json
```

### Auth and Config

```bash
mcporter auth <server | url> [--reset]
mcporter config list
mcporter config get <key>
mcporter config add <server>
mcporter config remove <server>
mcporter config import <path>
```

Config file location: `./config/mcporter.json` (override with `--config`).

### Code Generation

```bash
# Generate a CLI wrapper for an MCP server
mcporter generate-cli --server <name>

# Generate TypeScript types/client
mcporter emit-ts <server> --mode client
mcporter emit-ts <server> --mode types
```

---

## 12. Agents d'Aide / Helper Agents

The skill authoring system includes specialized AI agents to assist with various skill-related tasks.

### Comparator Agent (`agents/comparator.md`)

Compares two skills or two versions of the same skill. Identifies:
- Structural differences (frontmatter, sections, organization)
- Content gaps (missing sections, incomplete documentation)
- Trigger coverage differences
- Scoring and classification differences
- Recommendations for merging or improvement

### Analyzer Agent (`agents/analyzer.md`)

Post-hoc analysis of a skill for quality and completeness:
- Reads the complete SKILL.md
- Identifies missing elements (frontmatter fields, required sections)
- Suggests improvements
- Classifies the skill

### Grader Agent (`agents/grader.md`)

Evaluates and grades skill descriptions for quality and effectiveness:
- Scores the current description on trigger accuracy, clarity, conciseness
- Identifies ambiguous or misleading phrasing
- Suggests concrete improvements
- Can grade against a defined rubric

---

## 13. Références / References

### Schema Reference (`references/schemas.md`)

The complete schema reference for skill frontmatter, including:
- Field types and validation rules
- Naming conventions
- Metadata structure
- Compatibility specifications

### Installation Pitfalls (`references/installation-pitfalls.md`)

Comprehensive guide to common Hermes skill installation issues and resolutions:
- Hub connectivity problems
- Registry variations
- Naming conflicts
- Post-install verification
- Debugging failed installations

### Stale Reference Audit (`references/stale-reference-audit.md`)

Procedure for sweeping the skills library when a user catches a stale reference (dead paths, outdated model names, defunct providers). Covers: search pattern extraction, full-library sweep, hit classification, neutral replacement patterns, and prevention checklist. **Load this when fixing any outdated skill content.**

### Eval Viewer

The `eval-viewer/` directory contains:
- `viewer.html` — A self-contained HTML tool for reviewing eval results
- `generate_review.py` — Script to generate review pages programmatically

---

## 14. Pièges Communs / Common Pitfalls

### In Authoring

1. **Using `skill_manage(action='create')` for an in-repo skill.** It writes to `{SKILLS_DIR}/`, not the repo tree. Use `write_file` for in-repo creation.

2. **Leading whitespace before `---`.** The validator checks `content.startswith("---")`; any leading blank line or BOM fails validation.

3. **Description too generic.** Peer descriptions start with "Use when ..." and describe the *trigger class*, not the one task.

4. **Forgetting the author/license/metadata block.** Not validator-enforced, but every peer has it; omitting makes the skill look half-finished.

5. **Writing a skill that duplicates a peer.** Before creating, `ls skills/<category>/` and open 2-3 peers.

6. **Expecting the current session to see the new skill.** It won't. The skill loader is initialized at session start.

7. **Linking to skills that don't exist in-repo.** `related_skills: [some-user-local-skill]` works for you but breaks for other clones.

### In Architecture

8. **Confondre hub et repo.** Une skill peut exister dans un endroit et pas l'autre.

9. **`hermes skills list --all`** est le seul moyen fiable de voir TOUTES les skills disponibles (hub + repo).

10. **Les skills dans `skills/` sont dans `.gitignore` du repo** — pas de commit accidentel.

11. **Après un update Hermes**, les natives dans le repo peuvent changer. Tes copies dans `skills/` ne suivent pas.

### In Config Management

12. **Profile Deletion vs Skills Cleanup.** When asked to "clean up skills," do NOT delete profiles. The disabled directive controls skills. Profiles carry identity — deleting them is destructive.

13. **No Wildcard Support.** A literal `*` entry does NOT disable all skills.

14. **Updates May Re-enable Bundled Skills.** Bundled skills absent from both the disabled list AND the manifest WILL be re-created on update.

### In Packaging

15. **Packaging a skill that hasn't been validated.** The packager auto-runs validation, so this is caught, but save time by validating first.

16. **Including build artifacts.** The packager excludes `__pycache__`, `node_modules`, `evals/`, `*.pyc`, `.DS_Store` — but other artifacts may slip through.

### In Neutrality & Maintenance

17. **Hardcoding environment-specific values in skills.** Skills should never embed model names, provider configs, API endpoints, pricing, or gateway setups as if they were permanent facts. These change frequently. Instead: (a) reference the command to read live state (`hermes config`, `cat {CONFIG_PATH}`, `hermes status --all`), (b) use generic placeholders in examples (`{PROVIDER}/{MODEL_NAME}`, `<name>`), (c) if including historical benchmark/pricing data, mark it with a date and "verify current values" caveat. A skill that says "[USER_NAME] uses [MODEL_NAME]" or "the gateway is Telegram" will be wrong within weeks. A skill that says "run `hermes config` to see current model/provider" stays correct forever.

18. **Treating skill notes as source of truth for live config.** When answering user questions about "what's configured", always read the actual config files first — never rely on what a skill document says the config was. Skills document HOW to do things, not WHAT the current state is. If a skill contains a "Current Configuration" section, it's already stale. Replace it with a "Reading Current Configuration" section that points to live commands.

19. **Fixing one stale reference without sweeping the rest.** When a user catches a stale reference (dead path, outdated model name, defunct provider), do NOT stop at the one they pointed out. Immediately search the ENTIRE skills library (`search_files` across the skills directories) for the same pattern and fix all occurrences. A user who catches one stale reference has proven the skill library has a hygiene problem — the one they found is the symptom, not the disease. Sweep first, report after.

20. **Hardcoding paths/models/provider in SKILL.md.** Never write literal paths (`{HOME_DIR}Downloads/`), model names (`{MODEL_NAME}`), or provider names (`{PROVIDER}`) in skill content. Use `{VAR}` notation referencing `{VARS_PATH}`. When a value changes (e.g., model swap), only vars.md needs updating — not 15 skills. See `skills-governance-framework` for the full anti-hardcode rule.

---

## 15. Checklist de Vérification / Verification Checklist

### After Creating a Skill
- [ ] SKILL.md is at the correct path (user-local or in-repo)
- [ ] Frontmatter starts at byte 0 with `---`, closes with `\n---\n`
- [ ] `name` is lowercase + hyphens, ≤ 64 chars
- [ ] `description` is ≤ 1024 chars and starts with "Use when ..."
- [ ] `version`, `author`, `license`, `metadata.hermes.{tags, related_skills}` are present
- [ ] Total file ≤ 100,000 chars (aim for 8-15k)
- [ ] Structure: `# Title` → `## Overview` → `## When to Use` → body → `## Common Pitfalls` → `## Verification Checklist`
- [ ] Validated with `quick_validate.py`
- [ ] No duplication with existing peers in the same category
- [ ] Related skills reference in-repo skills (or explicitly user-local)
- [ ] **No hardcoded values** — paths, models, provider use `{VAR}` notation from `{VARS_PATH}`. Add `> **Variables:** See {VARS_PATH}` after frontmatter if variables are used.

### After Editing
- [ ] Validation re-run
- [ ] Committed (if in-repo)
- [ ] Description optimized via eval loop if needed

### After Packaging
- [ ] Validation passed
- [ ] `.skill` file created successfully
- [ ] Excluded artifacts confirmed absent

### After Hub Discovery
- [ ] Candidate skills ranked with relevance scores
- [ ] Install commands provided
- [ ] Installation pitfalls documented

---

## Scripts Summary

| Script | Purpose |
|--------|---------|
| `scripts/quick_validate.py` | Validate a skill's SKILL.md |
| `scripts/package_skill.py` | Package a skill into `.skill` file |
| `scripts/run_eval.py` | Evaluate skill against eval set |
| `scripts/run_loop.py` | Iteratively optimize skill description |
| `scripts/aggregate_benchmark.py` | Aggregate benchmark results |
| `scripts/generate_report.py` | Generate HTML report from loop output |
| `scripts/improve_description.py` | Improve description based on eval results |
| `scripts/utils.py` | Shared utilities (parse_skill_md) |

## License

This merged work incorporates content from multiple source skills. Individual source licensing:
- `anthropic/skill-creator`: Apache License 2.0 © Anthropic, PBC, 2026
- `skill-creator` (skills-management): MIT License
- All other sources: MIT License unless otherwise noted

See `LICENSE.txt` in this directory for the merged license terms.
