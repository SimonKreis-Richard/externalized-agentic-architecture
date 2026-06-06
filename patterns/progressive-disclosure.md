# Pattern: Progressive Disclosure

> Load depth on demand. Start shallow, go deep only when needed.

---

## Problem

Agents face a loading dilemma:
- Load everything → context bloat, attention degradation
- Load nothing → agent lacks necessary knowledge
- Load on first match → may load wrong skill, wasting tokens

## Solution

Three levels of disclosure:

| Level | Content | Token Cost | When |
|-------|---------|-----------|------|
| **L0: Catalog** | Name + description of all skills | ~3K tokens | Always (system prompt) |
| **L1: Full Content** | Complete skill documentation | Variable | On trigger match |
| **L2: References** | Specific files within a skill | Variable | On deep need |

The agent first scans L0 (the catalog) to detect matches. If a match is found, it loads L1 (full content). If L1 references sub-documents, it loads L2 (specific references) only when needed.

## Validation

- **Stigmem (Stanford, 2026):** Progressive disclosure = 50-87% token reduction vs eager loading
- **SRA-Bench (2026):** Targeted injection outperforms stuffing (60% → 45% with distractors)
- **HTTP Content Negotiation:** Progressive disclosure mirrors web API design (list → detail → raw)

## Anti-Patterns

- **Eager loading:** Loading full skill content "just in case"
- **Catalog bloat:** L0 catalog too large (>5K tokens) — defeats the purpose
- **Missing triggers:** Skills without trigger keywords won't be discovered at L0
- **Skipping levels:** Jumping to L2 without loading L1 — context gaps

---

## Sources

| Source | Relevance |
|--------|-----------|
| Stigmem (2026) | Progressive disclosure validated |
| SRA-Bench (2026) | Targeted > stuffed loading |
| HTTP RFC 7231 | Content negotiation pattern |
