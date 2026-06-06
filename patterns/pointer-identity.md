# Pattern: Pointer Identity

> Agent identity is a pointer to external structures, not an instruction manual embedded in context.

---

## Problem

Agent identity (personality, rules, behavioral reflexes) is typically defined in the system prompt. But system prompts are context — and context is finite. A verbose identity definition:
- Consumes tokens that could be used for reasoning
- Degrades in attention middle-position (Lost in the Middle)
- Cannot be updated without restarting the session
- Creates coupling between identity and runtime

## Solution

The identity layer contains only:
1. **Who the agent is** — name, role, relationship (1-3 sentences)
2. **How it behaves** — core reflexes, not procedures (3-5 rules)
3. **Where to find more** — pointers to external skill files

Everything else — procedures, workflows, detailed protocols — lives in external skill files loaded on demand.

## Architecture

```
┌─────────────────────────────────────┐
│  Identity Layer (~800-1200 tokens)  │  ← Always in context
│  • Name, role, relationship         │
│  • Core reflexes (3-5)              │
│  • Pointers to skills               │
├─────────────────────────────────────┤
│  Core Skills (~3-5K tokens each)    │  ← Always loaded
│  • Behavioral foundation            │
│  • Safety protocols                 │
│  • Universal reflexes               │
├─────────────────────────────────────┤
│  Specialist Skills (lazy)           │  ← Loaded on trigger
│  • Domain-specific procedures       │
│  • Deep analysis frameworks         │
│  • Configuration workflows          │
└─────────────────────────────────────┘
```

## Validation

- **Stigmem (Stanford, 2026):** Compact boot stub (<400 tokens) + lazy loading = 50-87% token reduction, 0 regressions
- **Persona Scaling Law (Huang et al., 2025):** Beyond anchoring, more persona text ≠ better behavior
- **"Don't Break the Cache" (Lumer et al., 2026):** Static identity prefix maximizes prompt caching hits

## Anti-Patterns

- **Identity as manual:** Putting procedures in the system prompt
- **Verbose personality:** Long adjective lists instead of energy-based behavioral guidance
- **Hardcoded references:** Pointing to specific file paths that change — use conventions instead

## Example

Instead of:
```
You are a helpful assistant. You always cite sources. When the user asks 
a question, you first check your knowledge base. If the information is 
not there, you search the web. You format responses in markdown. You 
use tables for comparisons. You...
```

Use:
```
You are Sam. You value truth over comfort. 
For procedures, load the appropriate skill.
```

---

## Sources

| Source | Relevance |
|--------|-----------|
| Stigmem (Stanford, 2026) | Lazy loading > preloading for instruction following |
| Huang et al. (2025) | Persona scaling law — diminishing returns |
| Lumer et al. (2026) | Cache stability requires static prefix |
| Liu et al. (2023) | Position matters — identity must be first |
