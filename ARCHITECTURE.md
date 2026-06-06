# Architecture — Technical Topology

> Information flow, component map, and the 3-layer stack for context-efficient AI agents.

---

## System Overview

```
┌──────────────────────────────────────────────────────┐
│                    USER INPUT                         │
└──────────────────────┬───────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────┐
│              ORCHESTRATION LAYER                      │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │
│  │   Routing    │  │   Direct    │  │  Parallel   │ │
│  │   (Goal)     │  │  Execution  │  │  Workers    │ │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘ │
└─────────┼────────────────┼────────────────┼─────────┘
          │                │                │
          ▼                ▼                ▼
┌──────────────────────────────────────────────────────┐
│              COGNITION LAYER                          │
│  ┌─────────────────────────────────────────────────┐ │
│  │  Context Window (processing workspace)          │ │
│  │  • Active reasoning                             │ │
│  │  • Working memory (hard cap)                    │ │
│  │  • Loaded skills (on demand)                    │ │
│  └─────────────────────────────────────────────────┘ │
└──────────────────────┬───────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────┐
│              EXTERNALIZATION LAYER                    │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────┐ │
│  │ Identity │ │  Skills  │ │  Memory  │ │Project │ │
│  │ (SOUL)   │ │ (Procs)  │ │ (Facts)  │ │Context │ │
│  └──────────┘ └──────────┘ └──────────┘ └────────┘ │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐            │
│  │ Backlog  │ │ Learnings│ │  Config  │            │
│  │ (Ideas)  │ │ (Insights│ │ (Prefs)  │            │
│  └──────────┘ └──────────┘ └──────────┘            │
└──────────────────────────────────────────────────────┘
```

## The 3-Zone Model

Information in the system exists in three zones:

| Zone | What | Lifecycle | Access |
|------|------|-----------|--------|
| **Zone 1: Context** | Active reasoning, loaded skills, working memory | Per-session | Every turn |
| **Zone 2: Durable** | Skills, memory, project context, config | Persistent | On demand |
| **Zone 3: Archive** | Session logs, learnings history, idea backlog | Append-only | On search |

**The flow:** Zone 3 → Zone 2 (capture/promote) → Zone 1 (load/reason) → Zone 2 (externalize) → Zone 3 (archive)

## Information Flow

### Inbound (User → Agent)

```
User message
  → Orchestrator (routing decision)
  → Direct / Goal-loop / Parallel workers
  → Load relevant skills (progressive disclosure)
  → Reason in context window
  → Externalize results
```

### Outbound (Agent → User)

```
Reasoning output
  → Format for user (concise, cited, actionable)
  → If durable: externalize to Zone 2
  → If archival: log to Zone 3
  → If traceable: add breadcrumb (Fil d'Ariane)
```

### Self-Maintenance (Agent → System)

```
Periodic audit (every ~10 sessions)
  → Check skill freshness
  → Check memory hygiene
  → Check config drift
  → Promote patterns (3-uses rule)
  → Prune stale artifacts
```

## Component Map

| Component | Zone | Purpose | Scientific Basis |
|-----------|------|---------|-----------------|
| **Identity Layer** | 1 (always) | Who the agent is | Stigmem — compact boot stub |
| **Core Skills** | 1 (always) | Universal reflexes, safety | SRA-Bench — 2-layer optimal |
| **Specialist Skills** | 2 (lazy) | Domain procedures | Stigmem — recall on demand |
| **Working Memory** | 1 (capped) | Durable facts (hard limit) | Cowan — 4±1 chunks |
| **Long-term Memory** | 2 (unlimited) | Skills, procedures | Clark & Chalmers — extended mind |
| **Episodic Memory** | 3 (searchable) | Session insights | Biological analogy |
| **Backlog** | 2-3 | Idea funnel | Funnel Cognition pattern |
| **Config** | 2 | System preferences | Stable across sessions |

## Token Budget Allocation

Based on Tian Pan (2026) 4-tier allocation model:

| Tier | Purpose | % of Context Window |
|------|---------|-------------------|
| **Static Anchors** | Identity + core skills + tools | 10-15% |
| **Retrieved Context** | Loaded specialist skills | 5-20% |
| **Conversation** | User messages + reasoning | 50-70% |
| **Scratch** | Working memory, tool output | 10-20% |

**Key constraint:** Static anchors must remain stable for prompt caching. Dynamic content (loaded skills, conversation) goes after the static prefix.

## Scaling Behavior

| Load Level | Core Skills | Specialist Skills | Total Context Usage |
|------------|------------|-------------------|-------------------|
| **Light** (simple query) | 3 loaded | 0 loaded | ~15-20% |
| **Medium** (research task) | 3 loaded | 1-2 loaded | ~25-35% |
| **Heavy** (complex project) | 3 loaded | 3-5 loaded | ~40-60% |
| **Maximum** (multi-domain) | 3 loaded | 5-8 loaded | ~60-80% |

---

## References

| Source | Relevance |
|--------|-----------|
| Tian Pan (2026) | Token budget allocation model |
| Clark & Chalmers (1998) | Extended mind — externalization zones |
| Cowan (2001) | Working memory capacity |
| SRA-Bench (2026) | 2-layer loading validation |
| Stigmem (2026) | Progressive disclosure |
| Lumer et al. (2026) | Cache stability — static prefix |
