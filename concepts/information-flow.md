# Information Flow: The Externalization Data Pipeline

> *This document describes the data pipeline that implements the Externalization Architecture. For the principles behind it, see [MANIFESTO.md](../MANIFESTO.md).*

---

## Everything is a Data Flow

Consider a real session trace:

```
Simon thinks → writes in Obsidian → Sam reads note
    → Sam breaks down task → delegates research to sub-agents
    → sub-agents search web → return findings → Sam synthesizes
    → Sam writes result to project/agents/ → pattern detected → brv_curate to ByteRover
    → procedure repeated 3 times → skillify → reusable skill
    → project context updated → AGENTS.md enriched
```

Every step is **input → process → externalize**. The brain (Sam) processes. The environment stores. The loop continues. The context window is the processing workspace — everything that can be externalized, is externalized.

This is Clark & Chalmers' **active externalism** in practice: the environment plays an *active, constitutive role* in cognition. The files, skills, and knowledge trees are not passive storage — they are *part of the cognitive system*, consulted every session, automatically endorsed.

---

## The Funnel: Four Stages

Information enters chaotic and leaves structured. Each stage reduces volume, increases signal.

### Stage 1: Chaotic Input
Raw, unfiltered, unstructured.
- User thoughts (typed or spoken)
- Web research results
- Sub-agent output
- File contents
- ByteRover query results
- Conversation context

No filtering at this stage. Everything is captured.

### Stage 2: Distillation (Sam, the Core Agent)
The agent processes raw input:
- Breaks down complex instructions
- Identifies what needs research vs. execution
- Delegates parallel work to sub-agents
- Finds patterns, connections, contradictions
- Decides what is durable vs. ephemeral

This is the *processing* stage. It happens in the context window.

### Stage 3: Categorization
Each output is routed to its proper external home:

| Output Type | Destination | Rule |
|---|---|---|
| Procedure (3rd use) | Skill | `skillify` → `0-custom-skills/` |
| Durable fact | `MEMORY.md` or ByteRover | User-approved, compact, stable |
| Project context | `AGENTS.md` | Lives with the project |
| Session output | `[project]/agents/YYYY-MM-DD_desc.ext` | Timestamped, auditable |
| Pattern or insight | ByteRover (brv_curate) | Hierarchical knowledge tree |
| Decision or push | ByteRover (brv_curate) | Pattern capture |
| Transient/tactical | Conversation only | Evaporates after session |

### Stage 4: Structured Knowledge
Durable, queryable, version-controlled.
- Skills (`0-custom-skills/` + `skills/`)
- ByteRover memory tree
- `MEMORY.md` + `USER.md`
- `data/*.json`
- Project `AGENTS.md` files
- Git history

This is the agent's **extended mind** — what survives a tool deletion, a machine wipe, a platform migration.

---

## The Recycling Loop

Context is too expensive to use once. The system recycles:

```
Session insight ──→ learnings-capture ──→ ByteRover or MEMORY.md
Procedure ×3 ──────→ skillify ───────────→ reusable skill  
Decision pattern ──→ brv_curate ─────────→ ByteRover entry
Skill overlap ─────→ skill-recycler ─────→ consolidation
Rich session ──────→ Sam proposes capture → user approves
```

This is automated and proactive. Sam initiates captures. Cron jobs run audits. The system compounds.

---

## The Local-First Paradigm

The cloud is transforming from *data custodian* to *inference provider*. AI is an API reaching into local data.

**Consequences**:
- Personal data stays in plain text on the local machine
- AI agents interact with local filesystems directly
- Multiple agents read the same local data
- Sync via Git — *data* syncs, *agent state* does not
- The cloud becomes a commodity inference layer; value accrues to the local data architecture

---

## Why Plain Text

Markdown + JSON, nothing else.

- **LLM-native**: Agents read and write text trivially
- **Queryable**: JSON is directly filterable
- **Versionable**: Git works natively. Diffs are meaningful
- **Portable**: Move to any OS, any machine, any agent
- **Survivable**: What lives in plain text outlives any tool

**The rule**: everything that can be text, is text. The only exceptions are media files referenced by path.

---

## The Extended Mind in Practice

| Extended Mind Concept | Implementation |
|---|---|
| **Active Externalism** | SOUL.md, MEMORY.md, skills are read every turn — they *are* the extended mind |
| **Cognitive Coupling** | Git-tracked files, ByteRover tree, Obsidian vault — accessible every session |
| **Functional Parity** | A MEMORY.md entry consulted every turn = functionally identical to internal memory |
| **Complementarity** | Context window + externalized skills + ByteRover = cognitive system larger than any single component |
| **Otto's Notebook** | The agent's files ARE its notebook. Constantly accessible. Automatically endorsed. |

---

**Version**: 4.0.0  
**Last updated**: 2026-05-17  
**Aligned with**: MANIFESTO.md v4.0.0

> *"The notebook qualifies as such because it is constantly and immediately accessible to Otto, and it is automatically endorsed by him."* — Clark & Chalmers, 1998
