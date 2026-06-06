# Pattern: Externalized Memory

> Durable knowledge lives in files, not in context. Context is for reasoning. Files are for remembering.

---

## Problem

LLMs have no persistent memory across sessions. Every session starts blank. The naive solution — stuffing facts into the system prompt — hits three walls:
1. **Capacity:** Context windows are finite (even 200K tokens fill fast)
2. **Attention:** Loaded facts compete with active reasoning for attention
3. **Staleness:** Facts loaded at session start may be irrelevant by session end

## Solution

A tiered memory architecture that mirrors biological cognition:

| Store | Analogy | Capacity | Access Pattern |
|-------|---------|----------|----------------|
| **Working Memory** | RAM | Hard cap (e.g., 2200 chars) | Every turn |
| **Long-term Memory** | Skills/Procedures | Unlimited | On demand |
| **Episodic Memory** | Session logs/search | Unlimited | On demand |
| **Reference Memory** | Structured data files | Unlimited | On demand |
| **Project Memory** | Per-project context | Unlimited | On demand |

**Routing rules:**
- Facts that reduce future steering → Working Memory (always injected)
- Procedures used repeatedly → Long-term Memory (skills)
- Session insights → Episodic Memory (learnings file)
- Structured data → Reference Memory (JSON/markdown files)
- Project-specific state → Project Memory (per-project files)

## Validation

- **Clark & Chalmers (1998):** External structures are constitutive parts of cognition
- **Cowan (2001):** Working memory limited to 4±1 chunks — hard cap is essential
- **Stigmem (2026):** Recall on demand outperforms preloading

## Anti-Patterns

- **Instructions in memory:** Memory files are data, not prompts. Behavioral rules belong in skills.
- **Unlimited working memory:** Without a hard cap, working memory grows until it displaces reasoning.
- **Stale entries:** Facts that are no longer true waste attention. Regular hygiene required.
- **Progress logs in memory:** Completed tasks are history, not durable facts.

---

## Sources

| Source | Relevance |
|--------|-----------|
| Clark & Chalmers (1998) | External structures = cognition |
| Cowan (2001) | Working memory capacity limit |
| Stigmem (2026) | Recall on demand > preloading |
