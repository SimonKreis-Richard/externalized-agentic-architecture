# Architecture — The Externalized Cognitive System v5.0

> *This document describes HOW the externalization data flow is implemented. For WHY, see [MANIFESTO.md](MANIFESTO.md).*

---

## 1. The Core Model: Cognition as Data Flow

Every interaction follows the same pattern:

```
Input → Process → Externalize
```

- **Input**: User instruction, web research, file context, session_search results, sub-agent returns
- **Process**: Sam (core agent) breaks down, delegates, synthesizes, decides
- **Externalize**: Output lands in a durable structure — file, skill, MEMORY.md, AGENTS.md, data/

The context window is the *processing workspace*. External structures are the *extended mind*. The goal is to minimize what stays in the workspace by maximizing what moves to durable structures.

This operationalizes Clark & Chalmers' Extended Mind Thesis (1998): the agent's cognitive system extends into its files, skills, and knowledge trees. These external structures are not just storage — they are *constitutive* parts of the cognitive system, consulted every session, endorsed automatically.

---

## 2. The Externalization Topology

Every piece of information has a *proper home* based on its nature and lifespan:

| Information Type | Home | Lifespan | Access Pattern |
|---|---|---|---|
| **Identity, tone, reflexes** | `SOUL.md` | Permanent (versioned) | Injected every turn |
| **Durable facts** | `MEMORY.md` | Permanent | `memory()` tool — queried as needed |
| **Procedural knowledge** | Skills (core + auto-detection) | Permanent | Loaded on demand via skill_view |
| **User profile** | `USER.md` | Permanent | `memory()` tool — injected |
| **Project context** | `AGENTS.md` (per project) | Project lifetime | Injected at session start |
| **Session output** | `[project]/agents/` | Project lifetime | Created, then referenced |
| **Cross-session knowledge** | session_search + memory | Permanent | `session_search()` for history queries |
| **Learned patterns** | Skills (via skillify) | Permanent | 3 uses → captured, frontmatter priority |
| **Domain knowledge** | `knowledge/oracle-hcm/` (RAG) | Permanent | Queried as reference |
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
│  Durable fact? → memory() tool → MEMORY.md                   │
│  Project context? → AGENTS.md                                │
│  Session output? → [project]/agents/                         │
│  Pattern/insight? → save as skill or memory                  │
│  Transient/tactical? → stays in conversation only            │
└───────────────────────────┬─────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    STRUCTURED KNOWLEDGE                      │
│  Skills (2 layers), MEMORY.md, AGENTS.md, data/*.json        │
│  All version-controlled. All portable. All survivable.       │
└─────────────────────────────────────────────────────────────┘
```

This is a **funnel**, not a bucket. Each stage reduces volume and increases signal. The agent is the distillation engine. The external structures are the permanent knowledge.

---

## 4. Two-Layer Architecture: Scaffolding vs. Engine

### Scaffolding (Disposable)
The tool hosting the agent — Hermes Agent. Provides model access, MCP tool execution, session management, cron scheduling, skill loading, sub-agent orchestration. It can be deleted and reinstalled without losing anything essential. It's a runtime, not a home.

### Engine (Persistent)
The plain-text files that define the agent's extended mind:
- `SOUL.md` — identity and behavioral contract
- `MEMORY.md` — durable facts (managed via `memory()` tool)
- `USER.md` — user profile and preferences
- Skills (2 layers) — externalized procedural memory
- `knowledge/oracle-hcm/` — domain RAG corpus (9 guides)
- `data/*.json` — structured personal data
- `AGENTS.md` — per-project context
- `.gap-tracker.md` — skills capability gaps
- `.recycler-log.md` — skill consolidation history

**Design rule**: everything that matters is a plain-text file. Nothing that defines the agent lives inside the scaffolding. The engine survives any tool migration.

---

## 5. Skills: Two-Layer Loading Architecture

Skills are the agent's **externalized procedural memory**. Loading is tiered by `priority:` in frontmatter YAML:

### Layer 1 — Core (14 skills, always loaded)

These are injected at every session start. They form the meta-system:

| Skill | Role | Priority |
|---|---|---|
| `system-architecture` | SSOT architecture manifest | core |
| `soul-management` | SOUL design & versioning | core |
| `skill-design` | Skill creation methodology | core |
| `skills-classifier` | Audit + priority classification | core |
| `search-tool-protocol` | Tool selection protocol | core |
| `delegation-agentique` | Dynamic delegation model | core |
| `web-research` | Web research patterns | core |
| `markitdown` | Document conversion | core |
| `hermes-system-maintenance` | System audit & cleanup | core |
| `autonomie-maintenance` | Autonomy orchestration | core |
| `hermes-agent` | Hermes agent skill (self-ref) | core |
| `skill-creator` | Skill creation with evals | core |
| `writing-plans` | Writing planning | core |
| `prism` | Unified analysis (5 sub-modes) | core |

**Rule**: core = meta/système/délégation/recherche/maintenance. Transverse utilities used in 50%+ of sessions.

### Layer 2 — Standard / Niche / One-Shot (~76 skills, auto-detection)

Loaded dynamically by Hermes via frontmatter name/description matching. The sub-tiers (standard/niche/one-shot) govern curation priority, not loading mechanism:

| Tier | Count | Examples |
|---|---|---|
| **Standard** | ~52 | `docker-management`, `codebase-inspection`, `docx`, `xlsx`, `planning-with-files` |
| **Niche** | ~13 | `blood-analyst`, `drug-discovery`, `fitness-nutrition`, `drawio-skill`, `kpi-hero` |
| **One-Shot** | ~10 | `omh-ralph`, `omh-triage`, `omh-deep-research` (oh-my-hermes family) |

**Total**: ~90 skills across 6 sources.

### Skill Source Directory Structure

```
skills/<source>/<name>/SKILL.md
```

| Source | Directory | Skills |
|---|---|---|
| `anthropic` | `skills/anthropic/` | 4 (excel-author, comps-analysis, dcf-model, 3-statement-model) |
| `bundled` | `skills/bundled/` | ~35 (plan, writing-plans, skill-creator, docx, xlsx, pdf, pptx, arxiv, etc.) |
| `community` | `skills/community/<repo>/` | ~20 (oh-my-hermes/10, super-hermes/5, planning-with-files, avoid-ai-writing, etc.) |
| `custom` | `skills/custom/` | 19 (system-architecture, delegation-agentique, prism, oracle-hcm-rag, etc.) |
| `hub` | `skills/hub/` | 6 (docker-management, scrapling, neuroskill-bci, fitness-nutrition, drug-discovery, one-three-one-rule) |
| `lobehub` | `skills/lobehub/` | 4 (ai-agent-generator, ai-prompts-assistant, blood-analyst, kpi-hero) |

**Disabled skills**: ~60 bundled skills disabled in `config.yaml` to avoid bloat.

**Classification tool**: `skills/custom/skills-classifier/classifier.py` — run in audit mode to generate reports at `skills-tools/classification-report.md`.

---

## 6. Sub-Agent Model: Orchestrator Pattern

Sub-agents are **empty shells**: base config only, no SOUL, no personality, no auto-loaded skills. All context is injected at call time. They execute and return results. They do not talk to the user.

This maps to the Extended Mind Thesis: sub-agents are parallel processing streams that extend the core agent's cognitive capacity. They process, then externalize their results back to the core. They are not independent agents — they are cognitive extensions.

**Configuration** (from config.yaml):
- `max_concurrent_children: 3`
- `max_spawn_depth: 3`
- `orchestrator_enabled: true`
- `subagent_auto_approve: true`
- `inherit_mcp_toolsets: true` — sub-agents inherit MCP tools (Tavily, Exa, Jina)
- `model: deepseek/deepseek-v4-flash` — same model as core
- All context injected at delegation time → no persistence, no memory
- Results reviewed critically by the core agent

**Rules**:
- Max 3 concurrent sub-agents
- All context injected at delegation time (explicit, no SOUL)
- Results reviewed critically by the core agent
- No personality, no memory, no persistence
- Spawn trees logged in `~/.hermes/spawn-trees/<id>/`

---

## 7. The Recycling Loop: Context as Compound Interest

The system doesn't just externalize — it recycles. Every cognitive effort compounds:

```
Session insight ──→ learnings-capture ──→ MEMORY.md or skill
Procedure ×3 ──────→ skillify ───────────→ reusable skill
Decision pattern ──→ save as skill or memory ───→ durable skill entry
Skill overlap ─────→ skill-recycler ─────→ consolidation
Rich session ──────→ propose capture ─────→ user approves
```

This is automated through **Hermes cron** (4 jobs, all every 180m):

| Cron Job | Skill Used | Scope |
|---|---|---|
| `system-audit` | `hermes-system-maintenance` | Full system cross-scan: config drift, profile SOUL alignment, skill bloat, JSON staleness, dotfiles completeness, OpenRouter cost monitoring |
| `soul-cross-check` | `soul-audit` | 5 checks: token count, paradigm coherence, contradictions, completeness, skill alignment |
| `skills-classifier-audit` | `skills-classifier` | Source folder correctness, quality, orphans, priority alignment |
| `hub-scout-scan` | `hub-scout` | 7 domains (Oracle HCM, Finance, Health, Productivity, Data, Architecture, Comms) — score candidates, update gap tracker |

**Plus SOUL triggers**:
- After 3 technical exchanges → re-inject warmth
- After rich session → propose learnings-capture
- After decision/push → save as skill or memory

**Critical dependency**: cron jobs only run when `hermes gateway` is up. Without the gateway, the recycling loop is a dormant schedule — zero actual autonomy.

---

## 8. Memory Architecture (Hermes Native)

Replaced ByteRover with Hermes's native memory system. No external memory server.

| Memory Type | Mechanism | Persistence | Purpose |
|---|---|---|---|
| **Durable Facts** | `memory()` tool → `MEMORY.md` | Permanent | Environment, conventions, quirks |
| **User Profile** | `memory()` tool → `USER.md` | Permanent | Identity, preferences, style |
| **Conversation History** | `session_search()` | Cross-session (searchable) | Recall past context |
| **Procedural Knowledge** | Skills (2 layers) | Permanent | Reusable workflows |
| **Domain RAG** | `knowledge/oracle-hcm/` | Permanent | Reference docs (9 guides) |
| **Personal Data** | `data/*.json` | Permanent | Structured domain knowledge |

**Config**:
- `memory_enabled: true`
- `user_profile_enabled: true`
- `memory_char_limit: 2200`
- `user_char_limit: 1400`
- `provider: builtin`

**Rules**: No automatic memory creation. The user decides what enters durable memory. Memory is an index — detail lives in skills, knowledge, or subdirectories. Target: compact, high-signal, no logs.

---

## 9. Technology Stack

Every choice follows the "externalize → survive" principle:

| Decision | Rationale |
|---|---|
| **OpenRouter as sole provider** | Single API gateway. Model-agnostic. No vendor lock-in. **Blocks anthropic/\*** (402/403 errors). |
| **DeepSeek V4 Flash as daily driver** | MoE architecture. Absurd cost-to-quality ratio for sustained agentic use. |
| **Pro models (<1% of calls)** | Only for edge cases (e.g., `minimax/minimax-m2.7` as fallback). |
| **Plain text everywhere** | Survives any tool. Git-native. LLM-readable. |
| **JSON for structured data** | Portable, queryable, deduplicated by design. |
| **Git as SSoT and backup** | SSH push to `github.com/SimonKreis-Richard/samantha`. Auto-push after modif. |
| **Hermes Agent as scaffolding** | Provides model access, MCP, cron, skills, sub-agents, memory. Disposable runtime. |
| **MCP Trinity: Tavily → Exa → Jina** | Priority-ordered. One tool per function. No overlap. |

### MCP Trinity — Search Pipeline

```
Priority 1: Tavily  → General web search, crawl, extract, map, research
Priority 2: Exa     → Semantic web search, full page fetch
Priority 3: Jina    → Read URL, parallel read, screenshot, classify, deduplicate,
                      search arXiv/SSRN/BibTeX, expand query, sort by relevance
```

**Hermes MCP plugins**:
- `tavily`: `tavily-mcp@latest` (via npx)
- `exa`: `exa-mcp-server` (via npx)
- `jina-ai`: `mcp-remote https://mcp.jina.ai/v1` (via npx)

**Jina also handles**: screenshots, classification, deduplication (images + strings), arXiv search, SSRN search, query expansion, relevance reranking, PDF extraction, page datetime guessing.

### Auxiliary Models (all via OpenRouter)

| Role | Model |
|---|---|
| Vision | `google/gemini-3.1-flash-lite` |
| Web extraction | `google/gemini-3.1-flash-lite` |
| Compression | `deepseek/deepseek-v4-flash` |
| Skills hub | `deepseek/deepseek-v4-flash` |
| Session search | `google/gemini-3.1-flash-lite` |
| Curator | `deepseek/deepseek-v4-flash` |
| Delegation | `deepseek/deepseek-v4-flash` |

### Skill Loading Configuration

```
skills:
  loading: lazy          # Skills loaded on demand, not at startup
  external_dirs: /home/simon/.hermes/skills/custom  # Custom skills directory
  template_vars: true    # Allow {{variables}} in skills
  guard_agent_created: true  # Protect agent-created content
```

---

## 10. Component Map (Runtime Structure)

```
~/.hermes/                              # Hermes runtime (git-tracked)
├── SOUL.md                             # Core identity (~1132 chars, v4.1)
├── config.yaml                         # Full Hermes configuration (576 lines)
├── .env                                # API keys (gitignored)
├── memories/
│   ├── MEMORY.md                       # Durable facts (index, compact)
│   └── USER.md                         # User profile
├── skills/
│   ├── anthropic/                      # 4 skills (author: Anthropic)
│   ├── bundled/                        # ~35 skills (Hermes bundled)
│   ├── community/<repo>/               # ~20 skills (community-maintained)
│   ├── custom/                         # 19 skills (Simon + Sam)
│   ├── hub/                            # 6 skills (hub-installed)
│   └── lobehub/                        # 4 skills (lobehub origin)
├── skills-tools/
│   ├── classification.json             # Classifier output (JSON)
│   └── classification-report.md        # Classifier output (Markdown)
├── knowledge/
│   └── oracle-hcm/                     # Domain RAG corpus (9 guides)
├── cron/
│   ├── jobs.json                       # 4 cron jobs configuration
│   └── output/<id>/                    # Cron execution reports
├── spawn-trees/<id>/                   # Sub-agent spawn tree logs
├── sessions/                           # Session history (retention: 30 days)
├── kanban.db                           # Kanban system state
├── state.db                            # Hermes state
├── .gap-tracker.md                     # Missing capabilities (hub-scout)
├── .recycler-log.md                    # Skill consolidation history
├── logs/
│   ├── agent.log                       # Agent operation log
│   ├── gateway.log                     # Gateway log
│   ├── errors.log                      # Error log
│   └── curator/                        # Curator run logs
└── .curator_backups/                   # Automated skill backups (keep: 5)

project/                                # Example project
├── AGENTS.md                           # Project context (auto-loaded)
├── notes/                              # Human-created
└── agents/                             # Agent-generated (YYYY-MM-DD_desc.ext)
    └── learnings/                      # Self-improvement captures
```

---

## 11. Fil d'Ariane — Every Externalization Creates a Traceable Navigation Point

Every externalized structure creates a node in the system's navigation graph. This "red thread" ensures no cognitive output is lost — it can always be traced back to its origin.

| Externalization | Creates Navigation Point | Traceable How? |
|---|---|---|
| **Session output** → `agents/` | Timestamped file (`YYYY-MM-DD_desc.ext`) | Searchable by date + description |
| **Durable fact** → `memory()` | Entry in `MEMORY.md` (keyed by § separator) | Readable via `memory()` tool |
| **Procedure** → skillify | `skills/<source>/<name>/SKILL.md` with version | Git history + frontmatter version |
| **Project context** → `AGENTS.md` | Per-project context file | Loaded at session start per workdir |
| **Research** → web search | Search result + extracted content | Tavily/Exa/Jina tool calls logged |
| **Sub-agent execution** → spawn tree | `spawn-trees/<id>/` with index + outputs | JSONL timeline of all sub-agent calls |
| **Cron audit** → report | `cron/output/<id>/` with timestamped `.md` | Readable output, linked in cron jobs.json |
| **Classification** → report | `skills-tools/classification-report.md` | Regenerable via classifier.py --audit |
| **Skill consolidation** → log | `.recycler-log.md` | Chronological, versioned |
| **Capability gaps** → tracker | `.gap-tracker.md` | Updated every hub-scout run |
| **Session history** → search | `session_search()` tool | Full-text search over past sessions |
| **Config** → YAML | `config.yaml` | Git-tracked, diffable |

**The rule**: every action that externalizes knowledge must also create or update a navigation entry. The system must be able to find what it externalized.

---

## 12. Information Flow Sequence

1. User instruction enters → Sam reads SOUL + MEMORY + AGENTS.md
2. Sam breaks down task → identifies what needs research, what needs execution
3. Sam delegates parallel work to empty-shell sub-agents with explicit context (max 3)
4. Sub-agents return results → Sam reviews critically, synthesizes
5. Sam externalizes: writes output to `agents/`, updates AGENTS.md if needed, saves as skill or memory, proposes skillify
6. Cron jobs run autonomously every 180m: system-audit, soul-cross-check, skills-classifier-audit, hub-scout-scan
7. Next session: richer context available through externalized knowledge
8. Every externalization creates a navigation point — nothing is lost, everything is traceable

---

## Version History

| Version | Date | Key Changes |
|---|---|---|
| 5.0.0 | 2026-05-19 | 2-layer skills (core = 14, auto-detection = ~76); orchestrator sub-agent model; real stack (OpenRouter + DeepSeek V4 Flash + MCP Trinity); Hermes native memory; component map aligned to actual runtime; cron jobs documented; Fil d'Ariane traceability added |
| 4.0.0 | 2026-05-17 | Initial externalization architecture |
| 3.0.0 | 2026-05-16 | SOUL-centric design |
| 2.0.0 | 2026-05-15 | Minimalist architecture |
| 1.0.0 | 2026-05-15 | Initial architecture |

---

**Version**: 5.0.0  
**Last updated**: 2026-05-19  
**Aligned with**: MANIFESTO.md v4.0.0 (Externalization Architecture)

> *"If, as we confront some task, a part of the world functions as a process which, were it done in the head, we would have no hesitation in recognizing as part of the cognitive process, then that part of the world IS part of the cognitive process."* — Clark & Chalmers, 1998
