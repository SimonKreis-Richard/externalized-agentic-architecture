# Information Flow: The Externalization Data Pipeline

> *This document describes the data pipeline that implements the Externalization Architecture. For the principles behind it, see [MANIFESTO.md](../MANIFESTO.md).*

---

## Everything is a Data Flow

Consider a real session trace — every step leaves a node in the **fil d'Ariane**:

```
┌─ Session Start ──────────────────────────────────────────────┐
│ [USER_NAME] thinks → writes in Obsidian → [AGENT_NAME] reads note      │
│   → [AGENT_NAME] breaks down task → delegates research to sub-agents   │
│   → sub-agents search web → return findings                  │
│   → [AGENT_NAME] synthesizes → writes result to project/agents/        │
│   → pattern detected → skillify → reusable skill             │
│   → project context updated → AGENTS.md enriched             │
│   → session trace appended to /traces/                       │
└─ Each arrow = a node in the trace ───────────────────────────┘
```

Every step is **input → process → externalize**. The brain ([AGENT_NAME]) processes. The environment stores. The loop continues. The context window is the processing workspace — everything that can be externalized, is externalized. Configuration variables use `{VAR}` notation — version-controlled, resolved at runtime from `configuration file` or `data/*.json`.

This is Clark & Chalmers' **active externalism** in practice: the environment plays an *active, constitutive role* in cognition. The files, skills, and knowledge trees are not passive storage — they are *part of the cognitive system*, consulted every session, automatically endorsed.

---

## The Funnel: Four Stages

Information enters chaotic and leaves structured. Each stage reduces volume, increases signal. **Each stage creates a named node in the fil d'Ariane trace.**

### Stage 1: Chaotic Input
Raw, unfiltered, unstructured.  
*Node type: `input`*
- User thoughts (typed or spoken)
- Web research results
- Sub-agent output
- File contents
- session_search results
- Conversation context

No filtering at this stage. Everything is captured.

### Stage 2: Distillation ([AGENT_NAME], the Core Agent)
The agent processes raw input.  
*Node type: `processing`*
- Breaks down complex instructions
- Identifies what needs research vs. execution
- Delegates parallel work to sub-agents
- Finds patterns, connections, contradictions
- Decides what is durable vs. ephemeral

This is the *processing* stage. It happens in the context window. Each decision taken here is logged as a trace node.

### Stage 3: Categorization
Each output is routed to its proper external home.  
*Node type: `routing`*

| Output Type | Destination | Rule |
|---|---|---|
| Procedure (3rd use) | Skill | `skillify` → `0-custom-skills/` |
| Durable fact | `data/*.json` or project data | User-approved, structured, queryable |
| Project context | `AGENTS.md` | Lives with the project |
| Session output | Project-based externalization (dossiers projets) | Timestamped, auditable |
| Pattern or insight | Project data or skill capture | Hierarchical knowledge tree |
| Decision or push | Project data or skill capture | Pattern capture |
| Transient/tactical | Conversation only | Evaporates after session |

Every routing action emits a trace node recording what was stored, where, and why.

### Stage 4: Structured Knowledge
Durable, queryable, version-controlled.  
*Node type: `storage`*
- Skills (`0-custom-skills/` + `skills/`)
- `data/*.json` (structured user data)
- `SOUL.md` (pointer)
- Project `AGENTS.md` files
- Git history

This is the agent's **extended mind** — what survives a tool deletion, a machine wipe, a platform migration.

---

## Le fil d'Ariane dans le pipeline

Each stage emits a **trace node** that is appended to the session's fil d'Ariane. The trace is a JSON-sequence file that records every operation:

```json
[
  {"t": "2026-05-19T10:00:01Z", "type": "input",     "desc": "[USER_NAME] wrote note in Obsidian: 'explore MCP protocol'"},
  {"t": "2026-05-19T10:00:03Z", "type": "processing","desc": "[AGENT_NAME] decomposed task into 3 research subtopics"},
  {"t": "2026-05-19T10:00:05Z", "type": "routing",   "desc": "Delegated subtopic 1 to sub-agent alpha"},
  {"t": "2026-05-19T10:00:07Z", "type": "routing",   "desc": "Delegated subtopic 2 to sub-agent beta"},
  {"t": "2026-05-19T10:00:09Z", "type": "input",     "desc": "Sub-agent alpha returned 4 search results"},
  {"t": "2026-05-19T10:00:11Z", "type": "processing","desc": "[AGENT_NAME] synthesized results → wrote summary"},
  {"t": "2026-05-19T10:00:13Z", "type": "routing",   "desc": "Wrote summary to project/agents/2026-05-19_mcp-exploration.md"},
  {"t": "2026-05-19T10:00:15Z", "type": "storage",   "desc": "Detected pattern → skillify → saved to 0-custom-skills/"},
  {"t": "2026-05-19T10:00:17Z", "type": "storage",   "desc": "Updated AGENTS.md with new context"}
]
```

This is the **observability layer** for the cognitive pipeline. It answers:
- **What happened?** Every operation is timestamped and described.
- **Why?** Each node carries the context of the decision.
- **Where did it go?** Routing nodes record the destination path.
- **How did we get here?** The chain of nodes reconstructs the full reasoning path.

The fil d'Ariane lives at `[project]/traces/YYYY-MM-DD_session-trace.json` and is rotated daily.

---

## Lazy Loading

Skills and memory are **not loaded at startup**. Loading everything on session init would:
- Exceed context windows
- Waste tokens on irrelevant knowledge
- Slow session start

Instead, the system loads only:
1. **Core context**: `SOUL.md`, `data/*.json` (if small) — always loaded (small, durable)
2. **Project context**: `AGENTS.md` of the current project
3. **On-demand skills**: loaded by `run_skill` tool or `session_search` matching
4. **Session trace**: written continuously, read on restart for continuity

When a need arises — a task, a question, a pattern match — the agent fetches the relevant skill or memory chunk. This is the **lazy loading** principle: load what you need, when you need it.

```python
# Pseudo-code: lazy loading in practice
def load_skill(name):
    if name not in loaded_skills:
        skill = read_file(f"0-custom-skills/{name}.md")
        loaded_skills[name] = skill
        log_trace("loading", f"Loaded skill {name}")
    return loaded_skills[name]
```

Lazy loading makes the system scalable: tens or hundreds of skills can exist without collapsing the context window.

---

## The Recycling Loop

Context is too expensive to use once. The system recycles:

```
Session insight ──→ learnings-capture ──→ data/*.json or skill
Procedure ×3 ──────→ skillify ───────────→ reusable skill  
Decision pattern ──→ save as skill or data ─────────→ durable entry
Skill overlap ─────→ skill-recycler ─────→ consolidation
Rich session ──────→ [AGENT_NAME] proposes capture → user approves
```

Each recycling action is a node in the fil d'Ariane, recording what was recycled and where.

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
| **Active Externalism** | SOUL.md (pointer), data/*.json, skills are read every turn — they *are* the extended mind |
| **Cognitive Coupling** | Git-tracked files, skills + data, Obsidian vault — accessible every session |
| **Functional Parity** | A data/*.json entry consulted every turn = functionally identical to internal memory — but more durable |
| **Complementarity** | Context window + externalized skills + data = cognitive system larger than any single component |
| **Otto's Notebook** | The agent's files ARE its notebook. Constantly accessible. Automatically endorsed. |
| **Fil d'Ariane** | Every session step leaves a trace node — the full reasoning path is reconstructable after the fact |
| **Lazy Loading** | Skills are loaded on demand, not at startup — keeping the context window lean and the knowledge base deep |

---

**Version**: 6.0.0  
**Last updated**: 2026-05-30  
**Aligned with**: MANIFESTO.md v6.0.0

> *"The notebook qualifies as such because it is constantly and immediately accessible to Otto, and it is automatically endorsed by him."* — Clark & Chalmers, 1998
