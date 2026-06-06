# Design Patterns Catalog

> 7 patterns for context-efficient AI agent architecture.
> Each pattern is validated by peer-reviewed research and grounded in cognitive science.

---

## Pattern Index

| # | Pattern | Core Insight | Key Validation |
|---|---------|-------------|----------------|
| 1 | [Pointer Identity](pointer-identity.md) | Identity is a pointer, not a manual | Stigmem (2026) |
| 2 | [Two-Layer Loading](two-layer-loading.md) | Core always + lazy on-demand | SRA-Bench (2026) |
| 3 | [Orchestrated Routing](orchestrated-routing.md) | Route to execution mode | Dochkina (2026) |
| 4 | [Externalized Memory](externalized-memory.md) | Files > context for durable knowledge | Clark & Chalmers (1998) |
| 5 | [Funnel Cognition](funnel-cognition.md) | Ideas → capture → store → harvest | Extended Mind + MVP |
| 6 | [Fil d'Ariane](fil-dariane.md) | Every decision is traceable | Ariadne + Git |
| 7 | [Progressive Disclosure](progressive-disclosure.md) | Load depth on demand | Stigmem (2026) |

## How to Use These Patterns

1. **Start with Pointer Identity** — minimal identity layer + skill pointers
2. **Add Two-Layer Loading** — separate core from specialist
3. **Implement Orchestrated Routing** — route tasks to execution modes
4. **Build Externalized Memory** — tiered memory architecture
5. **Wire up Funnel Cognition** — capture/store/harvest ideas
6. **Add Fil d'Ariane** — traceability across all artifacts
7. **Apply Progressive Disclosure** — load depth on demand

Each pattern builds on the previous. You don't need all 7 to start — but the system improves as you add them.

## Pattern Relationships

```
Pointer Identity ──→ Two-Layer Loading ──→ Progressive Disclosure
        │                     │
        └──→ Externalized Memory ──→ Funnel Cognition
                     │
                     └──→ Fil d'Ariane
                     
Orchestrated Routing (orthogonal — applies to execution, not storage)
```
