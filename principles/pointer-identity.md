# Principle: Pointer Identity

> An agent's identity is a pointer to external structure, not an instruction
> manual embedded in the prompt.
> **Family:** Context discipline · **Placement Rule:** Q2 (what belongs in the core) · **2026:** mostly user-space

---

## Problem

Identity — who the agent is, how it behaves — usually lives in the system prompt.
But the system prompt is context, and context is the scarce resource. A verbose
identity:

- spends the attention budget on description instead of reasoning,
- buries its own instructions in the low-attention middle of a long prompt,
- couples behavior to a blob you must restart the session to change.

## Principle

The identity layer holds only three things:

1. **Who** — name, role, relationship, in a sentence or two.
2. **How** — a handful of behavioral reflexes (principles, not procedures).
3. **Where** — pointers to the skills and files that hold everything else.

Procedures, workflows, and detailed protocols do not live here. They live in
skills, loaded on demand. The core is small and stable so that the periphery can
be large and fluid.

## Theoretical grounding

- **Validated by** — Anthropic, *Effective context engineering* (2025): system
  prompts should hit "the right altitude" — neither brittle hardcoded logic nor
  vague hand-waving — and strive for "the minimal set of information that fully
  outlines expected behavior." (Note: minimal ≠ short; it means high-signal.)
- **Validated by** — Liu et al., *Lost in the Middle* (2023): position matters.
  Identity belongs at the primacy position; pointers at recency. The middle
  degrades.
- **Engineering judgment** — keeping the core stable also protects prompt-cache
  hits, since a changing prefix invalidates the cache.

## Practical application (DIY: Hermes / OpenClaw / custom)

- Keep `SOUL.md` / the identity block to a tight budget (aim well under ~1.2K
  tokens). If it grows, the overflow is procedure — move it to a skill.
- Write reflexes as principles ("truth over comfort"), not rules ("always cite
  sources"). Principles survive contexts where rules break.
- Replace inline procedures with pointers: *"For X, load the X skill."*
- Never hardcode file paths that drift; point by convention (skill name, tag).

## User-space vs. platform-space

**Mostly yours.** Platforms give you a system-prompt slot and a skills loader,
but *how lean and pointer-shaped your identity is* remains your design choice.
This is a discipline, not a feature you can switch on.

## Anti-patterns

- **Identity as manual** — procedures stuffed into the system prompt.
- **Adjective soup** — long personality lists instead of a few behavioral reflexes.
- **Drifting pointers** — references to paths that move; point by convention.

## Sources

| Source | Label | Link |
|--------|-------|------|
| Anthropic — Effective context engineering (2025) | Validated by | https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents |
| Liu et al. — Lost in the Middle (2023) | Validated by | https://arxiv.org/abs/2307.03172 |
