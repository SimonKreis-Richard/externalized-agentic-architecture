# Skills — Quickstart Guide

> Skills are externalized procedural memory. This guide explains how to create, classify, and use them. For the full reference, see `skills-reference.md` in the repo root.

---

## How Skills Work

Every skill is a **SKILL.md** file with two parts:

1. **YAML frontmatter** — metadata that Hermes uses for auto-detection and loading
2. **Markdown body** — the actual procedure, instructions, and examples

```yaml
---
name: my-skill-name
priority: standard
description: What this skill does when activated.
---

# My Skill Name

## Context
When to use this skill...

## Procedure
1. Step one
2. Step two
3. Step three

## Examples
...
```

The `name` and `description` fields in frontmatter are what Hermes matches against when deciding whether to auto-load a skill.

---

## The Priority System

Every skill must declare a `priority:` tier in its frontmatter:

| Priority | Loading | When to Use |
|----------|---------|-------------|
| **core** | Always loaded at session start | Used in 50%+ of sessions. Meta-system skills only. |
| **standard** | Auto-detected by name/description match | Broad utility, regular workflows |
| **niche** | Auto-detected, lower weight | Domain-specific, loaded when topic matches |
| **one-shot** | Auto-detected, minimal weight | Rare or experimental. Candidate for recycling. |

**Only `priority: core` guarantees injection every session.** All other tiers rely on Hermes's dynamic frontmatter matching.

---

## The 0- Prefix Convention

Custom curated skills live in a dedicated directory with a `0-` prefix:

```
0-skills-curated/<name>/SKILL.md
```

The `0-` prefix ensures:
- **Visibility** — sorts first in directory listings. Custom skills are impossible to overlook.
- **Intentionality** — forces conscious curation. You don't accidentally accumulate skills here.
- **Classification** — structural separation from bundled/third-party skills in `skills/<source>/`.

Your SOUL should point `SKILLS_DIR` to this directory.

---

## Creating a New Skill — Step by Step

### 1. Apply the 3-Uses Rule

A procedure must be executed **3 times** before it qualifies for skillification. Track candidates naturally — no overhead. After the third repetition, proceed.

### 2. Create the Skill File

```bash
hermes skills create --name <skill-name>
```

Or manually:
```bash
mkdir -p 0-skills-curated/<skill-name>
```

### 3. Write the SKILL.md

```yaml
---
name: <skill-name>
priority: standard
description: One-sentence description of what this skill does.
---
```

Then write the markdown body with:
- **Context** — when to use this skill
- **Procedure** — step-by-step instructions
- **Examples** — concrete input/output examples
- **Edge cases** — what to do when things go wrong

### 4. Classify the Priority

Ask: "Will this be used in 50%+ of sessions?" If yes → `core`. Otherwise → `standard`, `niche`, or `one-shot` based on frequency.

### 5. Test and Iterate

Load the skill, run it on real tasks, refine the procedure. Skills are living documents.

---

## When NOT to Create a Skill

- **Behavioral rules** → belong in `SOUL.md` (always-on reflexes)
- **One-off procedures** → won't recur, don't skillify
- **Things the model should figure out** → context is sufficient

**Rule of thumb**: If a behavioral rule is important enough to be permanent and always-on, it belongs in `SOUL.md`. If it's a reusable procedure (3+ uses), it belongs in a skill.

---

## The Recycling Rule

Skills overlap and drift. Periodically run `skill-recycler` to detect:
- Duplicate procedures across skills
- Drift between related skills
- Stale one-shot skills that should be pruned

**Lifecycle**: `CREATE → USE → SKILLIFY → RECYCLE/CONSOLIDATE → PRUNE`

Design your system to resist skill bloat. The goal is not to have the most skills — it is to have the *right* skills: high-signal, no duplication, each earning its place through repeated use.

---

## Quick Reference

| What | Where |
|------|-------|
| Core skills | `0-skills-curated/<name>/SKILL.md` with `priority: core` |
| Custom skills | `0-skills-curated/<name>/SKILL.md` |
| Bundled skills | `skills/<source>/<name>/SKILL.md` |
| Skill format | YAML frontmatter + Markdown body |
| Minimum uses before skillify | 3 |
| Max active skills at once | 5-8 |

For the complete reference (all tiers, lifecycle details, what NOT to skillify), see `skills-reference.md`.
