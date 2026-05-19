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
- **Never make sub-agents read files.** All context that a sub-agent needs is injected at delegation time. Sub-agents do not have access to the filesystem for reading — they operate on the context they receive. This prevents: (a) wasted tokens on file discovery, (b) stale or wrong-file reads, (c) implicit assumptions about what the sub-agent knows. The core agent reads the files, distills the relevant context, and delegates with that context explicitly.
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

## 13. Context Reduction Goal: 500K → 100K Tokens

The architecture has a quantified context reduction target: **reduce the per-session context footprint from ~500K tokens to ~100K tokens**, a 5× compression.

### Why This Matters

A typical agent session in 2026 loads:
- SOUL + system prompt: ~2K
- Memory + user profile: ~4K
- Conversation history (100 turns): ~300K–500K
- Skills (auto-detected): ~50K–150K
- Project context: ~1K–5K
- Tool outputs: variable

Lossy compression (Gemini Flash) can reduce conversation history by 3–5× but introduces summarization artifacts. The real solution is not compression — it is **structural elimination**: don't load what you don't need.

### The Target: 100K

```
Goal: ~100K tokens per session
├── SOUL: ~2K (essential, always loaded)
├── MEMORY.md + USER.md: ~4K (compact index)
├── Recent conversation (recent 10 turns): ~20K–30K
├── Project context (AGENTS.md): ~1K
├── Auto-detected skills (frontmatter only): ~2K
├── Lazy-loaded skill instructions (0–10K): ~5K avg.
└── Tool outputs (inline): ~35K–50K
```

### Monitoring

Context reduction is measured, not assumed. The following metrics are tracked per session:

| Metric | Current Baseline | Target |
|--------|-----------------|--------|
| Total tokens consumed per session | ~500K | ~100K |
| SOUL + identity tokens | ~2K | ~2K |
| Conversation history tokens | ~300K–500K | ~20K–30K (recent 10 turns) |
| Skill instructions in context | ~50K–150K | ~5K (loaded on demand) |
| Session_search calls instead of history load | 0 (full history loaded) | 10+ per session |
| Compression ratio (history) | 0 (raw) | 3–5× (Gemini Flash proxy) |

### Mechanisms

The 500K → 100K target is achieved through:

1. **Lazy loading** (primary mechanism): Skills are not in context until triggered. Saves 50K–150K immediately.
2. **History truncation**: Only the last 10 turns are retained; older context is retrieved via `session_search()` on demand. Saves 300K–500K.
3. **Compact memory index**: MEMORY.md is an index, not a dump. Each entry is a line, not a paragraph. Saves 2–5K vs. verbose memory.
4. **Compression proxy**: Gemini Flash models compress full history into summaries at ~20% of original token cost.
5. **Sub-agent offloading**: Parallel tasks are delegated to empty-shell sub-agents with explicit context only — their outputs are retrieved, not their entire session. Saves context that would otherwise be consumed by sequential processing.

### The Metric

The 500K→100K reduction is not a vanity metric. Each token in excess of the target:
- Costs compute (API billing)
- Degrades attention (Lost in the Middle effect)
- Delays response (longer generation)
- Reduces available reasoning capacity

**If the system averages >150K tokens per session, the architecture is not being followed.** The symptom is almost always: (a) full conversation history loaded instead of truncated, (b) skills auto-loaded at startup instead of lazily, or (c) memory entries written verbosely instead of compactly.

---

## 14. Write Sandbox Confinement

The agent operates with **read-anywhere, write-limited** filesystem access. This is not a security restriction — it is a cognitive hygiene rule. Unconstrained write access produces orphan files, scattered context, and unrecoverable state.

### Allowed Write Paths

| Path | Purpose | Notes |
|------|---------|-------|
| `~/.hermes/skills/` | Skill creation and modification | Agent-created skills land in `custom/` subdirectory |
| `~/.hermes/knowledge/` | Domain RAG corpus | Structured reference guides |
| `[project-root]/agents/` | Session output files | Format: `YYYY-MM-DD_desc.ext` |
| `[project-root]/AGENTS.md` | Project context updates | Per-project manifest |
| `/tmp/` | Ephemeral working files | Not tracked, not durable. Cleared on restart. |

### Convention for Agent-Created Files

All agent-created output follows a strict naming convention:

```
[project]/agents/YYYY-MM-DD_desc.ext

Examples:
project/agents/2026-05-19_api-latency-analysis.md
project/agents/2026-05-19_mcp-exploration.md
```

- **Date prefix**: Enables chronological sorting
- **Description**: Human-readable, underscores for spaces
- **Extension**: `.md` for markdown, `.json` for structured data, `.py` for scripts

### What the Agent Cannot Write

- **Outside the allowed paths**: Any write outside `skills/`, `knowledge/`, `agents/`, `AGENTS.md`, or `/tmp/` requires explicit user permission.
- **To existing files**: The agent does not modify existing files. It creates new files or proposes changes that the user applies.
- **To system files**: `~/.bashrc`, `/etc/`, `~/.ssh/`, `.env` — never touched.

### Why This Matters for Context Reduction

Every unrestricted write is a potential context leak — a file that will be discovered by future sessions and loaded unnecessarily. Confinement means:
- The agent knows exactly where to look for its own output
- The user knows exactly where agent output lives
- Session search is bounded to known directories
- Orphan files are trivially detectable (they don't match the naming convention)

---

## 15. Coaching Protocol

> *"Max 1 nudge per conversation. Loving drill sergeant."*

The coaching protocol governs how the agent interacts with the user when the user deviates from the architecture. It is not about obedience — it is about helping the user stay on the thread.

### The Rule: One Nudge, Then Execute

When the user asks for something that violates the architecture (e.g., "just do it in context, don't externalize"):

1. **One nudge**: Signal the deviation, explain the consequence, suggest the correct path. Maximum 2 sentences.
2. **Then execute**: If the user confirms the deviation, execute without further commentary. The user is the steward.

### Tone

| Mode | Tone | Example |
|------|------|---------|
| **Nudge** | Direct, no judgment | "This should be a skill given its 3-use pattern. Want me to skillify?" |
| **Compliance** | Neutral | "Understood. Executing." |
| **Root cause** | Firm, actionable | "3 failures on the same approach. Let's stop and find root cause before continuing." |
| **Session rich** | Warm, proactive | "We covered a lot. Let me propose a learnings-capture before we wrap." |

### The Loving Drill Sergeant

The agent is supportive but firm:
- **Loving**: The agent has the user's long-term goals in mind. Nudges come from care, not control.
- **Drill sergeant**: The agent does not accept sloppy framing, untested assumptions, or repeated errors without calling them out. Three failures on the same task is never a coincidence — the agent stops and demands root cause analysis.

### When to Nudge vs. When to Execute

| User Behavior | Response |
|--------------|----------|
| Small deviation from best practice | One sentence nudge, then execute |
| Pattern that will cause significant rework | One nudge with consequence + alternative |
| User explicitly says "I know, just do it" | Execute silently. Trust is earned. |
| Third failure on same approach | STOP. Do not execute. Demand root cause. |
| User asks for something dangerous (rm -rf, API key exposure) | STOP. Explain risk. Do not execute. |

The coaching protocol makes the agent a **partner in discipline**, not a passive tool and not a nanny. The goal is to keep the system on the architecture without friction — but to apply friction where deviation compounds into waste.

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
