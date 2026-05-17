# Agent Design Constitution

> **A principled framework for AI agent design. Minimal, portable, reproducible.**  
> **Designed for Hermes Agent as the primary scaffolding, with full compatibility for Claude Code.**

This repository contains a set of principles, templates, and architectural decisions for designing persistent, portable AI agents that survive any tool change, machine wipe, or platform migration. It is a design methodology — not software.

---

## The Problem

AI agents are accumulating complexity faster than we can manage it:
- **36+ skills** that nobody maintains
- **7+ profile personalities** that drift out of sync
- **Automatic mode detection** that guesses wrong
- **Vendor-locked memory** that dies with the tool

Most users treat their AI agent like a smartphone they never clean — features pile up, context accumulates, and eventually the experience degrades. 

This framework is the opposite: a **minimal, maintainable, portable** architecture for a single AI identity. No bloat. No guessing. No drift.

---

## The Philosophy

> *"Do fewer things, better. Perfect the essential components."*

Read the full philosophy: **[MANIFESTO.md](MANIFESTO.md)**

The nine pillars:
1. **Radical Simplicity** — Remove over add. Defaults over custom.
2. **Explicit Control** — The user decides. The system executes.
3. **Determinism Over Heuristics** — Rules over model inference.
4. **Total Portability** — Plain text. Git. Rebuildable from scratch.
5. **Single Source of Truth** — One file, one truth. No duplication.
6. **Token Discipline** — Every word earns its place.
7. **Active Externalization** — Nothing in context that can live in a file.
8. **Data Preservation** — Never delete. Archive, move, merge.
9. **Reproducibility** — Rebuildable by one person, in one hour.

---

## What's in This Repo

| File | Purpose |
|------|---------|
| [`MANIFESTO.md`](MANIFESTO.md) | The constitution. Design principles, decision framework, anti-principles. |
| [`ARCHITECTURE.md`](ARCHITECTURE.md) | Technical architecture. Information flow, delegation model, component map. |
| [`SOUL-TEMPLATE.md`](SOUL-TEMPLATE.md) | The canonical SOUL template (~800 tokens). Identity, tone, behavioral reflexes. |
| [`config.yaml.template`](config.yaml.template) | Suggested Hermes Agent configuration — model, provider, terminal, skills. |
| [`concepts/example-agents.md`](concepts/example-agents.md) | Fictional example of an `AGENTS.md` project context file. |
| [`concepts/information-flow.md`](concepts/information-flow.md) | Deep dive: active externalization, data pipelines, local-first paradigm. |
| [`LICENSE`](LICENSE) | MIT license. |
| [`.gitignore`](.gitignore) | Standard ignores for OS and editor files. |

---

## Who This Is For

- **Power users** who want an AI partner with a persistent, portable identity
- **System architects** interested in agent design principles
- **Hermes Agent users** — this architecture is designed for Hermes as the primary scaffolding
- **Claude Code users** — fully supported as a secondary target; all templates and principles apply identically
- **Anyone** tired of their AI assistant accumulating complexity they didn't ask for

---

## How It Works

1. **One SOUL.** A single `SOUL.md` file defines the agent's identity, tone, and behavioral reflexes. No mode detection. No dynamic injection. Static, predictable, portable.

2. **No automatic skills.** Skills are cast manually, only when needed. Zero automatic skill loading. Zero automatic skill creation. Start with zero, add when proven.

3. **Sub-agents are empty shells.** They receive context at delegation time, carry no personality, and leave no residue. They execute. They don't relate.

4. **Everything is plain text.** Markdown for prose. JSON for structured data. Git for versioning. Nothing proprietary. Nothing vendor-locked.

---

## The Agent Identity

The architecture emerges from a single design constraint: the agent is the system's core component. It is defined by a static behavioral contract (`SOUL.md`) — not a configurable chatbot, not an interchangeable interface. The principles keep it clean and portable. The identity is defined by the user. The architecture is universal.

**The identity is personal. The principles are not.**

---

## License

MIT — take the principles, adapt them, build your own. The identity is personal. The architecture is universal.
