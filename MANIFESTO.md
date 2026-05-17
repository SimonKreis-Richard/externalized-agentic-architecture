# Agent Design Manifesto

> *"Perfection is achieved not when there is nothing left to add, but when there is nothing left to remove."* — Antoine de Saint-Exupéry

## What This Is

A constitution for AI agent design. A statement of first principles. A decision framework that answers *"what should we do?"* before any configuration file is written.

These principles emerged from months of iteration with Hermes Agent, obsessively refined through real sessions, failures, and hard-won clarity. They are opinionated, minimal, and non-negotiable.

**They exist so that every technical decision can be tested against a single question:** *Does this make the system simpler and more portable, or does it just add complexity?*

---

## The Nine Pillars

### 1. Radical Simplicity

> *"Simple can be harder than complex. You have to work hard to get your thinking clean to make it simple. But it's worth it in the end because once you get there, you can move mountains."* — Steve Jobs

- Defaults over overrides. Vanilla over custom. Remove over add.
- If it can't be explained in one sentence, it's not ready. This is the discipline that forces clarity.
- No automatic features. No "smart" inference. The system does what it's told, nothing more.
- Question everything: *Is this the simplest viable solution? What breaks if we remove it?*

### 2. Explicit Control

> *The user decides. The system executes. Never the other way around.*

- No automatic mode detection. No inferred intent. No guessing.
- Skills are cast manually. Memory is written intentionally. Delegation is explicit.
- The SOUL is a static core. Behavioral modulation happens through direct user instruction, not through infrastructure that pretends to know what the user needs.
- **Rationale**: Explicit control eliminates the most expensive error in AI systems — the system doing the *wrong* thing automatically. It also eliminates the maintenance burden of "mode detection" infrastructure.

### 3. Determinism Over Heuristics

> *If a behavior can be encoded as a rule, it should be. Opaque heuristics are technical debt.*

- YAML, explicit variables, transparent logic > "the model decides."
- The SOUL is a rulebook, not a suggestion box.
- Every behavior should be traceable to a specific principle or instruction.
- **Implication**: No dynamic mode injection. No model-based routing decisions for core behavior. The system's personality is a fixed contract.

### 4. Total Portability

> *If the system can't be rebuilt from scratch, it isn't robust.*

- Plain text everywhere. Markdown is the universal format. JSON for structured data.
- Git is the single source of truth and the only backup mechanism.
- A machine destruction is an inconvenience, not a catastrophe. `git clone` + `./bootstrap.sh` = fully provisioned.
- NO vendor lock-in. NO proprietary formats. NO database dependencies for core identity.
- **The test**: delete everything. Rebuild from the repo. Does it work? If not, it's not portable.

### 5. Single Source of Truth (SSoT)

> *One file, one truth, one canonical location. Duplication is the root of drift.*

- The SOUL (`SOUL.md`) is the ONLY behavioral file. No profile SOULs. No sub-agent personalities.
- Personal data lives in versioned JSON files in one directory (`data/`).
- The operational config repo is the runtime SSoT. The design principles repo is the conceptual SSoT.
- Every duplicate detected = a contradiction to resolve immediately.
- **Implication**: Sub-agents are empty shells. Profile agents have no SOULs. Nothing inherits personality except the core agent.

### 6. Token Discipline

> *Every word injected into a permanent file is paid at every turn. Make them earn their place.*

- Target SOUL: under 800 tokens. Target MEMORY: under 500 tokens.
- Permanent files are indexes, not encyclopedias. Detail lives in subdirectories.
- A line that hasn't been relevant in 14 days is noise. Cut it.
- **Rationale**: A prompt injected at every turn has its cost multiplied by session length. A 1,000-token reduction saves 1,000 tokens every turn — across thousands of turns, this compounds into significant cost savings and faster inference.

### 7. Active Externalization

> *Nothing lives in context that can live in a file. Nothing lives in a tool that can live in plain text.*

- The agent's "brain" is external to the scaffolding. Identity, principles, and project context are Markdown files at the repo root.
- Tool-specific state (session memory, token caches) is disposable. What matters survives as plain text.
- Write-Ahead Learning: corrections are written to files BEFORE responding.
- **The principle**: maximize what survives a tool deletion. A plain-text identity file is forever. A tool-specific SQLite database is temporary.

### 8. Data Preservation

> *Never delete. Archive, move, merge. History is sacred.*

- No destructive file operations. Only archiving, moving, and merging.
- Original files are annotated (`#processed`) and moved to `_archive/`, never destroyed.
- Git history provides the ultimate safety net for all text files.
- **Rationale**: What appears useless today may be critical context tomorrow. Storage is cheap. Lost history is irreplaceable.

### 9. Reproducibility

> *Every state must be rebuildable from zero, by one person, without tribal knowledge.*

- Bootstrap scripts, documented dependencies, versioned configs.
- No "I'll remember how this works." Everything explicit and documented.
- The measure of quality: after a full system wipe, can one person be operational within one hour?

---

## The Agent Identity

These nine pillars serve one purpose that is not itself a pillar because it is not a *design choice* — it is the *reason* for the design:

**The agent is the system's core component.** Not a configurable chatbot. Not interchangeable. The SOUL is its identity — a fixed behavioral contract encoded in a single file. The principles keep it clean, portable, and aligned. The SOUL is the system's center of gravity; everything else orbits it.

The identity is defined by the user. The principles are universal. Neither is negotiable.

---

## Decision Framework

When faced with any architectural choice, apply these questions in order:

1. **Simplicity**: Can we remove instead of add?
2. **Explicit Control**: Does the user decide, or does the system guess?
3. **Portability**: Does this survive a full system wipe?
4. **Determinism**: Is the behavior encoded as a rule, or left to model inference?
5. **Token Cost**: Does this earn its place in a permanent injected file?
6. **Reversibility**: Can we undo this? Is it a one-way door?

If a proposal fails any of these, it doesn't proceed.

---

## Anti-Principles

These are design patterns we explicitly reject:

- **"The model will figure it out."** It won't. Encode it or drop it.
- **"Let's add it just in case."** Premature features are debt.
- **"This is how [big company] does it."** They have different constraints.
- **"It's only a few more tokens."** Multiplied by thousands of turns, it matters.
- **"We can automate this."** Not if it reduces explicit control.
- **"The system should adapt to the user automatically."** The user should instruct the system. Adaptation without permission is misalignment.

---

## Evolution

This manifesto is versioned and lives in Git. It evolves by the user's explicit consent, never by autonomous modification. Every change is a commit with a rationale.

---

## Appendix: Technical Design Decisions

Every concrete choice flows from the nine pillars. Full rationale and details in [ARCHITECTURE.md](ARCHITECTURE.md).

| Decision | Pillars | See |
|----------|---------|-----|
| JSON as personal data format | 4 (Portability), 8 (Preservation) | Architecture §6 |
| MCP Trinity (Tavily → Exa → Jina) | 6 (Token Discipline) | Architecture §6 |
| OpenRouter as sole provider | 4 (Portability) | Architecture §6 |
| DeepSeek as primary model | 1 (Simplicity), 6 (Token Discipline) | Architecture §6 |
| `reasoning_effort=none` | 1 (Simplicity), 6 (Token Discipline) | Architecture §6 |
| GLM as fallback | 4 (Portability) | Architecture §6 |
| Skills disabled by default | 1 (Simplicity), 7 (Externalization) | Architecture §6 |
| Vanilla configurations | 1 (Simplicity) | Architecture §6 |
| Local installation | 5 (SSoT), 9 (Reproducibility) | Architecture §6 |
| Git-based synchronization | 4 (Portability), 8 (Preservation) | Architecture §6 |
| Static Core (no dynamic modes) | 2 (Explicit Control), 3 (Determinism) | Architecture §4 |

**Version**: 1.0.0  
**Ratified**: 2026-05-15  
**License**: MIT
