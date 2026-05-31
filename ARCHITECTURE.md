# Architecture — The Externalized Cognitive System v6.0.0

> *This document describes HOW the externalization data flow is implemented. For WHY, see [MANIFESTO.md](MANIFESTO.md).*

---

## 1. The Core Model: Cognition as Data Flow

Every interaction follows the same pattern:

```
Input → Process → Externalize
```

- **Input**: User instruction, web research, file context, session_search results, sub-agent returns
- **Process**: The core agent breaks down, delegates, synthesizes, decides
- **Externalize**: Output lands in a durable structure — file, skill, AGENTS.md, data/

The context window is the *processing workspace*. External structures are the *extended mind*. The goal is to minimize what stays in the workspace by maximizing what moves to durable structures.

This operationalizes Clark & Chalmers' Extended Mind Thesis (1998): the agent's cognitive system extends into its files, skills, and knowledge trees. These external structures are not just storage — they are *constitutive* parts of the cognitive system, consulted every session, endorsed automatically.

---

## 2. The Externalization Topology

Every piece of information has a *proper home* based on its nature and lifespan:

| Information Type | Home | Lifespan | Access Pattern |
|---|---|---|---|
| **Identity, tone, reflexes** | `SOUL.md` | Permanent (versioned) | SOUL as pointer to system |
| **Structured user data** | `data/*.json` | Permanent | Queried as needed |
| **Procedural knowledge** | Skills (curated custom, 0- prefix) | Permanent | Loaded on demand |
| **Project context** | `AGENTS.md` (per project) | Project lifetime | Injected at session start |
| **Session output** | `[project]/agents/` | Project lifetime | Created, then referenced |
| **Cross-session knowledge** | session_search | Permanent | `session_search()` for history queries |
| **Domain knowledge** | `knowledge/` (RAG) | Permanent | Queried as reference |
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
│                    DISTILLATION (core agent)                 │
│  Break down, find patterns, identify what's durable vs.      │
│  ephemeral, delegate parallel work to sub-agents             │
└───────────────────────────┬─────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    CATEGORIZATION                            │
│  Procedure? → save as skill                                  │
│  Durable fact? → data/*.json or SOUL.md                     │
│  Project context? → AGENTS.md                                │
│  Session output? → [project]/agents/                         │
│  Pattern/insight? → save as skill or project data            │
│  Transient/tactical? → stays in conversation only            │
└───────────────────────────┬─────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    STRUCTURED KNOWLEDGE                      │
│  SOUL (pointer), curated skills (0- prefix), project data    │
│  (data/*.json), AGENTS.md. All version-controlled.           │
│  All portable. All survivable.                               │
└─────────────────────────────────────────────────────────────┘
```

This is a **funnel**, not a bucket. Each stage reduces volume and increases signal. The agent is the distillation engine. The external structures are the permanent knowledge.

---

## 4. Two-Layer Architecture: Scaffolding vs. Engine

### Scaffolding (Disposable)
The tool hosting the agent. Provides model access, MCP tool execution, session management, skill loading, sub-agent orchestration. It can be deleted and reinstalled without losing anything essential. It's a runtime, not a home.

### Engine (Persistent)
The plain-text files that define the agent's extended mind:
- `SOUL.md` — identity pointer (not a manual)
- Skills (2 layers) — externalized procedural memory (curated custom directory, 0- prefix)
- `data/*.json` — structured user data
- `AGENTS.md` — per-project context

**Design rule**: everything that matters is a plain-text file. Nothing that defines the agent lives inside the scaffolding. The engine survives any tool migration.

---

## 5. Pointer-SOUL Architecture

The SOUL.md is not a manual — it is a **pointer to the system**. This is justified by working memory research: Cowan (2001) demonstrated human working memory holds ~4 chunks. An SOUL that tries to encode the entire system exceeds this limit and becomes noise.

**What the SOUL contains** (~800-1200 tokens, justified by Cowan's 4-chunk limit):
- Identity declaration (1 chunk)
- Communication contract (1 chunk)
- Externalization reflex (1 chunk)
- Key pointers to system components (1 chunk)

**What the SOUL does NOT contain**:
- Procedures (→ skills)
- Configuration (→ config.yaml via {VAR})
- Domain knowledge (→ data/*.json)
- History (→ GitHub version control)

The SOUL points to the system. The system is the knowledge. This separation ensures the SOUL stays within working memory limits while the full knowledge base remains accessible on demand.

---

## 6. Skills: Two-Layer Loading Architecture

Skills are the agent's **externalized procedural memory**. Loading is tiered by `priority:` in frontmatter YAML:

### Layer 1 — Core (3 skills, always loaded)

These are injected at every session start. They form the meta-system:

| Skill | Role | Priority |
|---|---|---|
| `system-architecture` | Architecture manifest, SOUL pointer system | core |
| `agent-config` | Configuration management, {VAR} conventions | core |
| `verification` | Validation, testing, quality assurance | core |

**Rule**: core = architecture/config/verification. Transverse utilities used in 50%+ of sessions.

This is justified by context window management literature: fewer core skills means less baseline context consumption, freeing capacity for on-demand skill loading. Three skills at ~500 tokens each consume ~1.5K tokens — negligible against the 100K target.

### Layer 2 — Custom Curated Skills (0- prefix, auto-detection)

Loaded dynamically by the agent framework via frontmatter name/description matching. Organized in a **curated custom directory using 0- prefix convention** for classification and visibility.

**0- prefix convention**:
- Skills are organized in a curated custom directory using 0- prefix convention for classification and visibility
- Problem: agent frameworks have data hoarding on skills with no granular disable — bundled skills accumulate without user control
- Solution: manually curated custom directory. Only skills that are actively used are maintained. The 0- prefix ensures curated skills appear first in directory listings

**Skill Source Directory Structure**:

```
skills/<source>/<name>/SKILL.md
```

```
skills/
└── curated/                          # Manually curated custom skills (0- prefix)
    ├── 0-custom-skills/              # User-created and actively maintained skills
    └── ...
```

**Total**: curated set of actively maintained skills. Quality over quantity.

---

## 7. Sub-Agent Model: Orchestrator Pattern

Sub-agents are **empty shells**: base config only, no SOUL, no personality, no auto-loaded skills. All context is injected at call time. They execute and return results. They do not talk to the user.

This maps to the Extended Mind Thesis: sub-agents are parallel processing streams that extend the core agent's cognitive capacity. They process, then externalize their results back to the core. They are not independent agents — they are cognitive extensions.

### Why Dynamic Delegation > Fixed Roles

The orchestrator pattern is not just a convenience — it is empirically superior to fixed-role assignment. Dochkina (2026) demonstrated that dynamic delegation, where the orchestrator assigns tasks based on the specific requirements of each request, outperforms static role assignment in both task completion rate and resource efficiency.

The mechanism is straightforward: a fixed-role system pre-assigns capabilities ("Agent A handles research, Agent B handles code"), which creates rigidity. When a task spans multiple domains or requires unexpected capabilities, the fixed system either forces the wrong agent to attempt it or requires complex handoff protocols. Dynamic delegation avoids this by letting the orchestrator analyze each task and compose the optimal sub-agent configuration at delegation time — injecting only the context and tools needed, nothing more.

This also aligns with the SRA-Bench findings (Su et al., 2026): pre-loading capabilities into agents wastes context. Dynamic injection of task-specific context is more efficient than static capability assignment.

**Configuration** (from config.yaml):
- `max_concurrent_children: 3`
- `max_spawn_depth: 3`
- `orchestrator_enabled: true`
- `subagent_auto_approve: true`
- `inherit_mcp_toolsets: true` — sub-agents inherit MCP tools
- `model: same MOE model as core` — model-agnostic — use the same model as the core agent
- All context injected at delegation time → no persistence, no memory
- Results reviewed critically by the core agent

**Rules**:
- Max 3 concurrent sub-agents
- All context injected at delegation time (explicit, no SOUL)
- **Never make sub-agents read files.** All context that a sub-agent needs is injected at delegation time. Sub-agents do not have access to the filesystem for reading — they operate on the context they receive. This prevents: (a) wasted tokens on file discovery, (b) stale or wrong-file reads, (c) implicit assumptions about what the sub-agent knows. The core agent reads the files, distills the relevant context, and delegates with that context explicitly.
- Results reviewed critically by the core agent
- No personality, no memory, no persistence

---

## 8. The Recycling Loop: Context as Compound Interest

The system doesn't just externalize — it recycles. Every cognitive effort compounds:

```
Session insight ──→ save as learnings data
Procedure ×3 ──────→ save as reusable skill
Decision pattern ──→ save as skill or project data
Skill overlap ─────→ consolidation
Rich session ──────→ propose capture ─────→ user approves
```

Recycling is triggered by the core agent during sessions — no external scheduler required.

---

## 9. Memory Architecture: Disabled by Design

Memory is intentionally **disabled**. This is a deliberate architectural choice, not a limitation.

### Why Memory Is Disabled

Memory compaction (as implemented by most agent frameworks) suffers from five critical problems:

1. **Context window waste**: Memory compaction captures low-importance data alongside high-importance data. The compaction algorithm cannot reliably distinguish signal from noise, so it loads everything — wasting precious context window tokens on irrelevant entries.

2. **Hidden state**: The `memory()` tool creates hidden state that is not version-controlled. You cannot diff it, review its history, or roll it back. Changes to memory are invisible until they surface in a session.

3. **Latency**: External memory providers (vector databases, cloud-hosted stores, semantic search APIs) add network round-trips to every memory operation. A local JSON file has zero latency — it is read from disk in microseconds. For a system that queries structured data on every session, this difference compounds.

4. **Portability**: Memory providers are external services with APIs that change, pricing models that evolve, and availability that is not guaranteed. A JSON file is plain text — it works on any machine, any OS, any tool, forever. It survives provider shutdowns, API deprecations, and platform migrations. It is Git-native by default.

5. **Simpler alternatives exist**: Critical information externalizes cleanly into SOUL (for identity and reflexes — manual, granular control) and JSON files (for structured user data — queryable, version-controlled, zero-latency, fully portable). GitHub provides backup and versioning for the entire architecture.

### What Replaces Memory

| Purpose | Mechanism | Why Better |
|---|---|---|
| Identity & reflexes | SOUL.md (pointer) | Always visible, version-controlled |
| Structured user data | `data/*.json` | Queryable, version-controlled, granular |
| Procedural knowledge | Skills (0- prefix curated) | Loaded on demand, not in context |
| Project context | AGENTS.md (per project) | Project-scoped, clear lifecycle |

### GitHub as Backup and Versioning

All architecture files are backed by Git/GitHub. This provides:
- **Version history**: every change is tracked with full diff capability
- **Backup**: remote repository survives local failures
- **Rollback**: any architectural change can be reverted
- **Collaboration**: the architecture is shareable and forkable

Memory creates the illusion of persistence while actually creating maintenance debt. Explicit file-based externalization is more work upfront but eliminates entire categories of silent failures.

---

## 10. Technology Stack

Every choice follows the "externalize → survive" principle:

| Decision | Rationale |
|---|---|
| **Chinese open-source MOE models** | Best cost-to-quality ratio for sustained agentic use. The Chinese AI ecosystem (DeepSeek, Xiaomi, Qwen, Moonshot, MiniMax, StepFun, Zhipu) is engaged in an aggressive price war, subsidizing inference costs to gain market share. This creates an arbitrage opportunity: consumers access frontier-quality models at a fraction of Western pricing. Combined with MoE architecture (fewer active parameters per query = cheaper tokens without proportional quality loss), these models dominate the intelligence-per-dollar frontier. Refer to benchmarks: [GDPval-AA](https://artificialanalysis.ai/evaluations/gdpval-aa), [PinchBench](https://pinchbench.com), τ²-Bench, [Artificial Analysis cost curves](https://artificialanalysis.ai/?cost=intelligence-vs-cost) for model selection. No model loyalty — benchmark performance determines selection. |
| **Pro/large models (<1% of calls)** | Reserved for edge cases requiring maximum reasoning capacity. |
| **Plain text everywhere** | Survives any tool. Git-native. LLM-readable. |
| **JSON for structured data** | Portable, queryable, deduplicated by design. |
| **GitHub as SSoT and backup** | Version-controlled remote repository. Auto-push after modification. All architecture files tracked. |
| **Agent framework as scaffolding** | Provides model access, MCP, skills, sub-agents. Disposable runtime. |
| **MCP Trinity: Tavily → Exa → Jina** | Priority-ordered. One tool per function. No overlap. |

**Auxiliary models (optional)**: Use task-appropriate models for vision, web extraction, etc. — reference [Artificial Analysis intelligence-vs-cost curves](https://artificialanalysis.ai/?cost=intelligence-vs-cost) for optimal selection.

### MCP Trinity — Search Pipeline

```
Priority 1: Tavily  → General web search, crawl, extract, map, research
Priority 2: Exa     → Semantic web search, full page fetch
Priority 3: Jina    → Read URL, parallel read, screenshot, classify, deduplicate,
                      search arXiv/SSRN/BibTeX, expand query, sort by relevance
```

### Skill Loading Configuration

```
skills:
  loading: lazy          # Skills loaded on demand, not at startup
  external_dirs: <curated-custom-directory>  # Curated skills with 0- prefix
  template_vars: true    # Allow {VAR} notation in skills
```

---

## 11. Component Map (Runtime Structure)

```
~/.agent/                               # Agent runtime (git-tracked)
├── SOUL.md                             # Core identity pointer (~800-1200 tokens)
├── config.yaml                         # Full configuration
├── .env                                # API keys (gitignored)
├── skills/
│   └── curated/                        # Manually curated custom skills (0- prefix)
│       └── 0-custom-skills/            # User-created and actively maintained
├── data/
│   └── *.json                          # Structured user data (version-controlled)
├── spawn-trees/<id>/                   # Sub-agent spawn tree logs
├── sessions/                           # Session history (retention: 30 days)
├── kanban.db                           # Kanban system state
├── state.db                            # State
├── logs/
│   ├── agent.log                       # Agent operation log
│   ├── gateway.log                     # Gateway log
│   └── errors.log                      # Error log

project/                                # Example project
├── AGENTS.md                           # Project context (auto-loaded)
├── notes/                              # Human-created
└── agents/                             # Agent-generated (YYYY-MM-DD_desc.ext)
    └── learnings/                      # Self-improvement captures
```

---

## 12. Fil d'Ariane — Every Externalization Creates a Traceable Navigation Point

Every externalized structure creates a node in the system's navigation graph. This "red thread" ensures no cognitive output is lost — it can always be traced back to its origin.

| Externalization | Creates Navigation Point | Traceable How? |
|---|---|---|
| **Session output** → `agents/` | Timestamped file (`YYYY-MM-DD_desc.ext`) | Searchable by date + description |
| **Durable fact** → `data/*.json` | Entry in structured JSON file | Readable, queryable, version-controlled |
| **Procedure** → skill | `skills/<source>/<name>/SKILL.md` with version | Git history + frontmatter version |
| **Project context** → `AGENTS.md` | Per-project context file | Loaded at session start per workdir |
| **Research** → web search | Search result + extracted content | Tavily/Exa/Jina tool calls logged |
| **Sub-agent execution** → spawn tree | `spawn-trees/<id>/` with index + outputs | JSONL timeline of all sub-agent calls |
| **Classification** → report | Curated skill directory | Regenerable via audit |
| **Session history** → search | `session_search()` tool | Full-text search over past sessions |
| **Config** → YAML | `config.yaml` | Git-tracked, diffable |

**The rule**: every action that externalizes knowledge must also create or update a navigation entry. The system must be able to find what it externalized.

---

## 13. Information Flow Sequence

1. User instruction enters → the core agent reads SOUL + AGENTS.md
2. The core agent breaks down task → identifies what needs research, what needs execution
3. The core agent delegates parallel work to empty-shell sub-agents with explicit context (max 3)
4. Sub-agents return results → the core agent reviews critically, synthesizes
5. The core agent externalizes: writes output to `agents/`, updates AGENTS.md if needed, saves as skill or project data
6. Next session: richer context available through externalized knowledge
7. Every externalization creates a navigation point — nothing is lost, everything is traceable

---

## 14. Context Reduction Goal: 500K → 100K Tokens

The architecture has a quantified context reduction target: **reduce the per-session context footprint from ~500K tokens to ~100K tokens**, a 5× compression.

### Why This Matters

A typical agent session in 2026 loads:
- SOUL + system prompt: ~2K
- SOUL pointer + data files: ~2K
- Conversation history (100 turns): ~300K–500K
- Skills (auto-detected): ~50K–150K
- Project context: ~1K–5K
- Tool outputs: variable

Lossy compression can reduce conversation history by 3–5× but introduces summarization artifacts. The real solution is not compression — it is **structural elimination**: don't load what you don't need.

### The Target: 100K

```
Goal: ~100K tokens per session
├── SOUL: ~2K (essential, always loaded)
├── SOUL pointer + data/*.json: ~2K (compact, on-demand)
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
| Compression ratio (history) | 0 (raw) | 3–5× |

### Mechanisms

The 500K → 100K target is achieved through:

1. **Lazy loading** (primary mechanism): Skills are not in context until triggered. Saves 50K–150K immediately.
2. **History truncation**: Only the last 10 turns are retained; older context is retrieved via `session_search()` on demand. Saves 300K–500K.
3. **Compact SOUL**: SOUL as pointer, not manual. Saves tokens on every turn.
4. **Compression proxy**: Models compress full history into summaries at ~20% of original token cost.
5. **Sub-agent offloading**: Parallel tasks are delegated to empty-shell sub-agents with explicit context only — their outputs are retrieved, not their entire session. Saves context that would otherwise be consumed by sequential processing.

### The Metric

The 500K→100K reduction is not a vanity metric. Each token in excess of the target:
- Costs compute (API billing)
- Degrades attention (Lost in the Middle effect)
- Delays response (longer generation)
- Reduces available reasoning capacity

**If the system averages >150K tokens per session, the architecture is not being followed.** The symptom is almost always: (a) full conversation history loaded instead of truncated, (b) skills auto-loaded at startup instead of lazily, or (c) unnecessary data in context.

---

## 15. Write Sandbox Confinement

The agent operates with **read-anywhere, write-limited** filesystem access. This is not a security restriction — it is a cognitive hygiene rule. Unconstrained write access produces orphan files, scattered context, and unrecoverable state.

### Allowed Write Paths

| Path | Purpose | Notes |
|------|---------|-------|
| `~/.agent/skills/` | Skill creation and modification | Agent-created skills land in curated directory |
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

- **Outside the allowed paths**: Any write outside `skills/`, `agents/`, `AGENTS.md`, or `/tmp/` requires explicit user permission.
- **To existing files**: The agent does not modify existing files. It creates new files or proposes changes that the user applies.
- **To system files**: `~/.bashrc`, `/etc/`, `~/.ssh/`, `.env` — never touched.

### Why This Matters for Context Reduction

Every unrestricted write is a potential context leak — a file that will be discovered by future sessions and loaded unnecessarily. Confinement means:
- The agent knows exactly where to look for its own output
- The user knows exactly where agent output lives
- Session search is bounded to known directories
- Orphan files are trivially detectable (they don't match the naming convention)

---

## 16. {VAR} Notation System — Version-Controlled Configuration

Configuration uses `{VAR}` notation for variables, providing a single source of truth:

```
# In config files and skills:
model: {DEFAULT_MOE_MODEL}
provider: {DEFAULT_PROVIDER}
research_priority: {SEARCH_TOOL_PRIORITY}
```

**Principle**: All configurable values are declared as `{VAR}` in templates and resolved at runtime from a single config file. This ensures:
- Version-controlled single source of truth
- No duplicated configuration across files
- Easy migration between environments
- Clear documentation of what's configurable

Variables are defined in `config.yaml` and resolved when skills or templates are loaded.

---

## 17. Coaching Protocol

> *"Max 1 nudge per conversation. Loving drill sergeant."*

The coaching protocol governs how the agent interacts with the user when the user deviates from the architecture. It is not about obedience — it is about helping the user stay on the thread.

### The Rule: One Nudge, Then Execute

When the user asks for something that violates the architecture (e.g., "just do it in context, don't externalize"):

1. **One nudge**: Signal the deviation, explain the consequence, suggest the correct path. Maximum 2 sentences.
2. **Then execute**: If the user confirms the deviation, execute without further commentary. The user is the steward.

### Tone

| Mode | Tone | Example |
|------|------|---------|
| **Nudge** | Direct, no judgment | "This should be a skill given its 3-use pattern. Want me to save it as a skill?" |
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

## 18. Reasoning Effort Economics — The Low Sweet Spot

> *"A little reasoning helps a lot. A lot of reasoning hurts."*

### The Finding

Reasoning effort on MOE models does **not** cost proportionally more. The blended price is typically flat across none, low, high, and max effort levels. The difference is in **output quality and token efficiency**:

| Effort | Quality Index | vs None | Tokens |
|--------|--------------|---------|--------|
| None | baseline | — | Minimal |
| **Low** | **~+15%** | **Best value** | Efficient |
| High | +26% | Good for complex tasks | Moderate |
| Max | +27% | Marginal gain, overthinking risk | **Explodes** |

**Key insight**: High→Max gives only +1% quality but triggers *overthinking* — the model wastes the thinking budget on redundant exploration. Performance scales linearly from none to high, but cost (in tokens) scales exponentially from high to max.

### Why "none" Is Not Always Better

Setting reasoning to `none` is not always optimal. The relationship is nuanced:
- **Low reasoning captures ~80% of the none→high gain** at minimal token overhead
- For non-trivial tasks, `low` consistently outperforms `none` with negligible cost difference
- The sweet spot depends on task complexity: simple ops → `none`, most work → `low`, complex reasoning → `high`

### Decision Rule

| Use Case | Effort | Rationale |
|----------|--------|-----------|
| **Default** | `low` | Captures ~80% of none→high gain at minimal token overhead |
| **Trivial ops** (status checks, ls) | `none` | Overthinking wastes tokens for zero gain |
| **Complex reasoning** | `high` | When the task genuinely benefits from thinking through alternatives |
| **Never** | `max` | Marginal gain (+1%), overthinking triggers, token cost explodes |

### Why This Matters

- **Low reasoning is free performance.** 15-20% quality gain at same cost.
- **High reasoning is cheap performance.** 26% gain at marginal token overhead.
- **Max reasoning is wasteful.** Same quality as High but 3-10× the thinking tokens.

---

## 19. Model Selection Benchmarks

When selecting models, reference these benchmarks for empirical data:

| Benchmark | URL | What It Measures |
|---|---|---|
| **GDPval-AA** | [artificialanalysis.ai/evaluations/gdpval-aa](https://artificialanalysis.ai/evaluations/gdpval-aa) | Quality-adjusted performance per cost |
| **PinchBench** | [pinchbench.com](https://pinchbench.com/?view=graphs&score=average) | Agent capability benchmarks |
| **τ²-Bench** | τ²-Bench | Task completion and reasoning benchmarks |
| **Artificial Analysis** | [artificialanalysis.ai](https://artificialanalysis.ai/?cost=intelligence-vs-cost) | Intelligence-per-dollar curves |

**Recommendation**: Chinese open-source MOE models offer the best cost-efficiency for agentic workloads. Use these benchmarks to validate selection rather than relying on provider marketing claims.

---

## Version History

| Version | Date | Key Changes |
|---|---|---|
| 6.0.0 | 2026-05-30 | Anonymized design guide; Pointer-SOUL architecture; memory disabled by design; 3 core skills; {VAR} notation; 0- prefix convention; benchmark references; cron removed; specific model names removed |
| 5.1.0 | 2026-05-19 | §16 Reasoning Effort Economics — low sweet spot, Apple 'Illusion of Thinking', diminishing returns confirmed |
| 5.0.0 | 2026-05-19 | 2-layer skills; orchestrator sub-agent model; component map aligned to actual runtime |
| 4.0.0 | 2026-05-17 | Initial externalization architecture |
| 3.0.0 | 2026-05-16 | SOUL-centric design |
| 2.0.0 | 2026-05-15 | Minimalist architecture |
| 1.0.0 | 2026-05-15 | Initial architecture |

---

**Version**: 6.0.0  \
**Last updated**: 2026-05-30  \
**Aligned with**: MANIFESTO.md (Externalization Architecture)

> *"If, as we confront some task, a part of the world functions as a process which, were it done in the head, we would have no hesitation in recognizing as part of the cognitive process, then that part of the world IS part of the cognitive process."* — Clark & Chalmers, 1998
