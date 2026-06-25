# Project Context — The Bounded Agent

> **Type:** Public design-framework repository
> **Status:** Active rewrite (June 2026)

## What this repo is

A design framework for people configuring open-source agent frameworks (Hermes,
OpenClaw, custom harnesses). It translates the context-engineering thesis —
context is a finite attention budget — into a configuration decision discipline:
the **Placement Rule** and eight principles branching from it.

It is **not** a research paper, a benchmark, or a public dump of anyone's private
config. It claims no novelty about the underlying constraint, only about the
translation into DIY configuration decisions and the human-in-the-loop framing.

## Layout

```
MANIFESTO.md      → trade-offs as value pairs (no justification; that's in principles/)
README.md         → premise, what's borrowed vs. ours, how to read
concepts/         → the-placement-rule.md (the spine) + theory essays
principles/       → the 8 principles (problem / principle / grounding / DIY practice / platform note / anti-patterns)
ARCHITECTURE.md   → how the principles compose
references/        → bibliography; every source verified and labeled
```

## Source policy (non-negotiable)

Every citation must be opened and verified before it ships. Three labels only:
**Validated by** (direct LLM evidence), **Consistent with** (cog-sci analogy,
not proof), **Engineering judgment** (convergent practice, no paper). The
previous version shipped fabricated citations; the rewrite removed them and
records the removal in `references/bibliography.md`. Do not reintroduce an
unverifiable source under any framing.

## Conventions

- 100% English.
- Each principle states which side of the user-space / platform-space line it falls on.
- Prose over bullet-spam; tables where they earn their place.
