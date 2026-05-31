# Skills Reference — Public Template v6.0

> *Skills are externalized procedural memory. This document is a generic template for configuring your own 2-layer skill system with a curated custom directory. Adapt it to your domain, stack, and agent identity.*

---

## 1. Core Concept: Two-Layer Skill Architecture

Skills are the agent's **externalized procedural memory**. They live outside the context window and are loaded on demand. The system has two loading layers:

| Layer | Loading Mechanism | Purpose | Examples |
|-------|------------------|---------|----------|
| **Layer 1 — Core** | Always injected at session start | Architecture, configuration, verification | `system-architecture`, `agent-config`, `verification` |
| **Layer 2 — Auto-Detect** | Dynamically loaded via frontmatter name/description matching | Domain-specific procedures, niche workflows, one-shot tasks | Code analysis, document conversion, data analysis |

**Principle**: Core skills = used in 50%+ of sessions. Everything else lives in Layer 2 and is pulled in by Hermes when the frontmatter matches the current task.

Skills live in `0-custom-skills/<name>/SKILL.md` (curated) or `skills/<source>/<name>/SKILL.md` (bundled) and are version-controlled like all other engine files.

---

## 2. Frontmatter: The `priority:` Field

Every skill has YAML frontmatter. The critical field is `priority:`:

```yaml
---
name: my-skill-name
priority: core         # Layer 1 — always loaded at session start
# priority: standard   # Layer 2 — auto-detected, medium curation priority
# priority: niche      # Layer 2 — auto-detected, low curation priority
# priority: one-shot   # Layer 2 — auto-detected, candidate for recycling
description: What this skill does when activated.
---
```

**Only `priority: core` guarantees injection every session.** All other tiers use Hermes's dynamic frontmatter matching — the agent loads the skill when it detects a semantic match between its task and the skill's name/description.

---

## 3. Recommended Core Skills (Bring Your Own)

Your Layer 1 should cover meta-system operations. The 3 core skills:

| Skill | Purpose |
|-------|---------|
| `system-architecture` | Architecture manifest, SOUL pointer system, design decisions |
| `agent-config` | Configuration management, {VAR} conventions, model selection |
| `verification` | Validation, testing, quality assurance |

Install hub skills via:
```bash
hermes skills install --yes <skill-identifier>
```

---

## 4. Curated Custom Directory with 0- Prefix Convention

### The Problem

Agent frameworks encourage data hoarding on skills with no granular disable mechanism. Skills accumulate, overlap, and become noisy. There is no structural distinction between a carefully curated custom skill and a randomly installed one.

### The Solution

A curated custom directory with `0-` prefix for classification and visibility.

### Convention

```
0-custom-skills/<name>/SKILL.md
```

The `0-` prefix ensures:
- **Visibility**: sorts first in any directory listing — custom skills are impossible to overlook
- **Intentionality**: forces conscious curation. You don't accidentally accumulate skills here.
- **Classification**: structural separation from bundled/third-party skills in `skills/<source>/`

### When to Create a Custom Skill

1. A procedure has been executed 3+ times (3-uses rule)
2. The procedure is genuinely non-obvious (not just a checklist)
3. The skill passes the Apple Principle: "If this were removed, would the system break?"

### When NOT to Create

- Behavioral rules that belong in SOUL.md (always-on reflexes)
- One-off procedures that won't recur
- Things the model should figure out from context

---

## 5. Creating Your Own Skills

Use the `skillify` workflow — either via the `skill-creator` skill or directly:

### The 3-Uses Rule

**A procedure must be executed 3 times before it qualifies for skillification.** Premature skill creation is context debt. Track candidates mentally or in a simple file; after the third repetition, skillify.

### The Recycling Rule

When skills overlap, consolidate them. Run `skill-recycler` periodically to detect:
- Duplicate procedures across skills
- Drift between related skills
- Stale one-shot skills that should be pruned

### Skillification Workflow

```
1. Execute procedure 3 times (track naturally, no overhead)
2. Run skillify: hermes skills create --name <name>
3. Move to 0-custom-skills/<name>/SKILL.md
4. Classify priority: core/standard/niche/one-shot
5. Load, test, iterate
```

---

## 6. Basic Hermes Installation Commands

```bash
# Install Hermes Agent (via pip or npm — check current docs)
pip install hermes-agent          # or equivalent

# Create a new skill
hermes skills create --name my-skill

# Install skills from hub
hermes skills install --yes anthropics/skills/skills/skill-creator
hermes skills install --yes anthropics/skills/skills/brainstorming

# List installed skills
hermes skills list

# Update skills
hermes update
```

Full documentation: [hermes-agent.nousresearch.com/docs](https://hermes-agent.nousresearch.com/docs)

---

## 7. What NOT to Skillify

These patterns belong in `SOUL.md` (the behavioral contract), not in a skill:

| Pattern | Why It's Permanent |
|---------|--------------------|
| Communication style & tone | Universal reflex, not a procedure |
| Token discipline (be concise) | Core identity, not situational |
| Delegation rules | Fundamental operating principle |
| Externalization reflex | The cornerstone of the architecture |
| Search tool priority order | Always the same sequence |
| Fact-checking requirement | Always-on quality gate |
| Scope & confinement rules | Safety boundary, not situational |

**Rule of thumb**: If a behavioral rule is important enough to be permanent and always-on, it belongs in `SOUL.md`. If it's a reusable procedure (3+ uses), it belongs in a skill.

---

## 8. Skill Lifecycle

```
CREATE ──→ USE ──→ SKILLIFY ──→ RECYCLE/CONSOLIDATE ──→ PRUNE
```

| Stage | Action | Trigger |
|-------|--------|---------|
| **CREATE** | Write a skill for a new procedure | Anticipated reuse (before 3-uses, it's a note) |
| **USE** | Load and execute the skill | Task matches frontmatter |
| **SKILLIFY** | Promote to durable skill | After 3 successful uses |
| **RECYCLE/CONSOLIDATE** | Merge overlapping skills | skill-recycler detects duplication or drift |
| **PRUNE** | Archive or delete stale skills | No uses in 30 days, or consolidated into another |

Design your system to resist skill bloat. The goal is not to have the most skills — it is to have the *right* skills: high-signal, no duplication, each earning its place through repeated use.

---

> *"The notebook qualifies as such because it is constantly and immediately accessible to Otto, and it is automatically endorsed by him."* — Clark & Chalmers, 1998

**Version**: 6.0.0
**Last updated**: 2026-05-30
**License**: MIT
