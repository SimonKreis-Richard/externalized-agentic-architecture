# Skills Reference — Public Template v5.0

> *Skills are externalized procedural memory. This document is a generic template for configuring your own 2-layer skill system. Adapt it to your domain, stack, and agent identity.*

---

## 1. Core Concept: Two-Layer Skill Architecture

Skills are the agent's **externalized procedural memory**. They live outside the context window and are loaded on demand. The system has two loading layers:

| Layer | Loading Mechanism | Purpose | Examples |
|-------|------------------|---------|----------|
| **Layer 1 — Core** | Always injected at session start | Meta-system, delegation, research, maintenance | `system-architecture`, `skill-design`, `skill-creator`, `web-research` |
| **Layer 2 — Auto-Detect** | Dynamically loaded via frontmatter name/description matching | Domain-specific procedures, niche workflows, one-shot tasks | Code analysis, document conversion, data analysis |

**Principle**: Core skills = used in 50%+ of sessions. Everything else lives in Layer 2 and is pulled in by Hermes when the frontmatter matches the current task.

Skills live in `skills/<source>/<name>/SKILL.md` and are version-controlled like all other engine files.

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

Your Layer 1 should cover meta-system operations. Install or create equivalents of:

| Skill | Purpose |
|-------|---------|
| `skill-design` | Methodology for creating and modifying skills |
| `skill-creator` | Skill creation with automated evals |
| `skill-recycler` | Detect overlap, consolidate, prune |
| `system-architecture` | SSOT for your system design |
| `search-tool-protocol` | Tool selection for web/API search |
| `web-research` | Efficient web research patterns |
| `hermes-system-maintenance` | Unified system audit and cleanup |

Install hub skills via:
```bash
hermes skills install --yes <skill-identifier>
```

---

## 4. Creating Your Own Skills

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
3. Classify priority: core/standard/niche/one-shot
4. Load, test, iterate
```

---

## 5. Basic Hermes Installation Commands

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

## 6. What NOT to Skillify

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

## 7. Skill Lifecycle

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

**Version**: 5.0.0
**Last updated**: 2026-05-19
**License**: MIT
