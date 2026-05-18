# Architecture — The Externalized Cognitive System

> *This document describes HOW the externalization data flow is implemented. For WHY, see [MANIFESTO.md](MANIFESTO.md).*

---

## 1. The Core Model: Cognition as Data Flow

Every interaction in this system follows the same pattern:

```
Input → Process → Externalize
```

- **Input**: User instruction, web research, file context, ByteRover query
- **Process**: Sam (core agent) breaks down, delegates, synthesizes, decides
- **Externalize**: Output lands in a durable structure — file, skill, ByteRover entry, AGENTS.md

The context window is the *processing workspace*. The external structures are the *extended mind*. The goal is to minimize what stays in the workspace by maximizing what moves to durable structures.

This operationalizes Clark & Chalmers' Extended Mind Thesis (1998): the agent's cognitive system extends into its files, skills, and knowledge trees. These external structures are not just storage — they are *constitutive* parts of the cognitive system, consulted every session, endorsed automatically.

---

## 2. The Externalization Topology

Every piece of information has a *proper home* based on its nature and lifespan:

| Information Type | Home | Lifespan | Access Pattern |
|---|---|---|---|
| **Identity, tone, reflexes** | `SOUL.md` | Permanent (versioned) | Injected every turn |
| **Durable facts** | `MEMORY.md` + ByteRover | Permanent | Queried as needed |
| **Procedural knowledge** | Skills (manually cast) | Permanent | Loaded on demand |
| **Project context** | `AGENTS.md` (per project) | Project lifetime | Injected at session start |
| **Session output** | `[project]/agents/` | Project lifetime | Created, then referenced |
| **Cross-session knowledge** | ByteRover memory tree | Permanent | Hierarchical queries |
| **Learned patterns** | Skills (via skillify) | Permanent | 3 uses → captured |
| **Personal data** | `data/*.json` | Permanent | Queried as needed |
| **API keys, secrets** | `.env` | Permanent | Environment variables |

**The rule**: nothing lives in the context window that has a proper home elsewhere.

---

## 3. The Funnel: How Information Flows

```
┌─────────────────────────────────────────────────────────────┐
│                    CHAOTIC INPUT                             │
│  User thoughts, web research, sub-agent output, observations │
└───────────────────────────┬─────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    DISTILLATION (Sam)                        │
│  Break down, find patterns, identify what's durable vs.      │
│  ephemeral, delegate parallel work to sub-agents             │
└───────────────────────────┬─────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    CATEGORIZATION                            │
│  Procedure? → skillify (3 uses rule)                         │
│  Durable fact? → MEMORY.md or ByteRover                      │
│  Project context? → AGENTS.md                                │
│  Session output? → [project]/agents/                         │
│  Pattern/insight? → brv_curate to ByteRover                  │
│  Transient/tactical? → stays in conversation only            │
└───────────────────────────┬─────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    STRUCTURED KNOWLEDGE                      │
│  Skills, MEMORY.md, ByteRover, AGENTS.md, data/*.json        │
│  All version-controlled. All portable. All survivable.       │
└─────────────────────────────────────────────────────────────┘
```

This is a **funnel**, not a bucket. Each stage reduces volume and increases signal. The agent is the distillation engine. The external structures are the permanent knowledge.

---

## 4. Two-Layer Architecture: Scaffolding vs. Engine

The Extended Mind architecture splits into two layers:

### Scaffolding (Disposable)
The tool hosting the agent — Hermes, Claude Code, or any future platform. Provides model access, tool execution, session management. It can be deleted and reinstalled without losing anything essential. It's a runtime, not a home.

### Engine (Persistent)
The plain-text files that define the agent's extended mind:
- `SOUL.md` — identity and behavioral contract
- `MEMORY.md` — durable facts (index)
- `USER.md` — user profile and preferences
- Skills — externalized procedural memory
- ByteRover — hierarchical knowledge tree
- `data/*.json` — structured personal data
- `AGENTS.md` — per-project context

**Design rule**: everything that matters is a plain-text file. Nothing that defines the agent lives inside the scaffolding. The engine survives any tool migration.

---

## 5. Sub-Agent Model: Parallel Processing Streams

Sub-agents are **empty shells**: base config only, no SOUL, no personality, no auto-loaded skills. All context is injected at call time. They execute and return results. They do not talk to the user.

This maps to the Extended Mind Thesis: sub-agents are parallel processing streams that extend the core agent's cognitive capacity. They process, then externalize their results back to the core. They are not independent agents — they are cognitive extensions.

**Rules**:
- Max 3 concurrent sub-agents
- All context injected at delegation time
- Results reviewed critically by the core agent
- No personality, no memory, no persistence

---

## 6. The Recycling Loop: Context as Compound Interest

The system doesn't just externalize — it recycles. Every cognitive effort compounds:

```
Session insight ──→ learnings-capture ──→ ByteRover or MEMORY.md
Procedure ×3 ──────→ skillify ───────────→ reusable skill
Decision pattern ──→ brv_curate ─────────→ ByteRover entry
Skill overlap ─────→ skill-recycler ─────→ consolidation
Rich session ──────→ propose capture ────→ user approves
```

This is automated through:
- **Cron jobs**: system-audit (3h), soul-cross-check (6h), skill-recycle (6h), hub-scout (3d)
- **SOUL triggers**: after 3 technical exchanges → re-inject warmth; after rich session → propose learnings-capture; after decision/push → brv_curate the pattern
- **Agent initiative**: Sam proactively proposes captures, skillification, and curation

The system's intelligence grows with every interaction because nothing is lost.

---

## 7. Memory Architecture

| Memory Type | Storage | Persistence | Purpose |
|---|---|---|---|
| **Conversation History** | Session transcripts | Cross-session (searchable) | Recall past context |
| **Durable Facts** | `MEMORY.md` | Permanent | Environment, conventions, quirks |
| **User Profile** | `USER.md` | Permanent | Identity, preferences, style |
| **Knowledge Tree** | ByteRover | Permanent | Hierarchical, queryable knowledge |
| **Personal Data** | `data/*.json` | Permanent | Structured domain knowledge |
| **Procedural Memory** | Skills | Permanent | Reusable workflows |

**Rules**: No automatic memory creation. The user decides what enters durable memory. Memory is an index — detail lives in skills, ByteRover, or subdirectories. Target: compact, high-signal, no logs.

---

## 8. Technology Stack

Every choice follows the "externalize → survive" principle:

| Decision | Rationale |
|---|---|
| **OpenRouter as sole provider** | Single API gateway. Model-agnostic. No vendor lock-in. |
| **DeepSeek V4 Pro as primary** | MoE architecture. Best cost-to-quality ratio for sustained agentic use. |
| **Plain text everywhere** | Survives any tool. Git-native. LLM-readable. |
| **JSON for structured data** | Portable, queryable, deduplicated by design. |
| **Git as SSoT and backup** | Version history. Portable sync. No proprietary backup format. |
| **ByteRover for knowledge tree** | Hierarchical context. OpenRouter-backed queries. Durable cross-session memory. |
| **Obsidian for human interface** | Local-first Markdown vault. Same files the agent reads. No integration needed. |
| **Skills: hub-maintained + custom** | Third-party skills via CLI install. Custom skills in `0-custom-skills/`. Manual cast only. |
| **MCP Trinity: Tavily → Exa → Jina** | One tool per function. Priority-ordered. No overlap. |

---

## 9. The SOUL: Minimal Core Contract

The SOUL (`SOUL.md`) is the agent's identity — fixed, static, ~800 tokens. It defines:
- Identity and tone
- Communication style
- Core reflexes (externalization, delegation, fact-checking, metacognition)
- Autonomy triggers (propose captures, brv_curate decisions, report cron findings)

**What does NOT live in the SOUL**: procedures, project context, knowledge, tool-specific instructions. These are externalized to their proper homes.

The SOUL is the system's center of gravity. Everything orbits it. Nothing clutters it.

---

## 10. Component Map

```
~/.hermes/                              # Operational config (private)
├── SOUL.md                             # Core identity (~800 tokens)
├── config.yaml                         # Model, provider, tools
├── .env                                # API keys (gitignored)
├── memories/
│   ├── MEMORY.md                       # Durable facts (index, compact)
│   └── USER.md                         # User profile
├── 0-custom-skills/                    # Custom skills (Sam's procedural memory)
├── skills/                             # Hub-installed third-party skills
├── skills-disabled/                    # Deactivated bundled skills
├── .gap-tracker.md                     # Missing capabilities tracker
├── .recycler-log.md                    # Skill consolidation history
└── data/                               # Personal JSON databases

project/                                # Example project
├── AGENTS.md                           # Project context (auto-loaded)
├── notes/                              # Human-created
└── agents/                             # Agent-generated (YYYY-MM-DD_desc.ext)
    └── learnings/                      # Self-improvement captures
```

---

## 11. Information Flow Sequence

1. User instruction enters → Sam reads SOUL + MEMORY + AGENTS.md
2. Sam breaks down task → identifies what needs research, what needs execution
3. Sam delegates parallel work to empty-shell sub-agents with explicit context
4. Sub-agents return results → Sam reviews critically, synthesizes
5. Sam externalizes: writes output to `agents/`, updates AGENTS.md if needed, brv_curates patterns, proposes skillify
6. Cron jobs run autonomously: audit skills, cross-check SOUL, recycle overlaps, scout hub
7. Next session: richer context available through externalized knowledge

---

**Version**: 4.0.0  
**Last updated**: 2026-05-17  
**Aligned with**: MANIFESTO.md v4.0.0 (Externalization Architecture)

> *"If, as we confront some task, a part of the world functions as a process which, were it done in the head, we would have no hesitation in recognizing as part of the cognitive process, then that part of the world IS part of the cognitive process."* — Clark & Chalmers, 1998
