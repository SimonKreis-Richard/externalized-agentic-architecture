# Externalization Architecture — A Manifesto

> *"Where does the mind stop and the rest of the world begin?"* — Clark & Chalmers, *The Extended Mind* (1998)

## What This Is

A design constitution for AI agent systems built on a single insight: **cognition is a data flow.** Every thought — biological or artificial — is input, processing, output. The bottleneck is never processing power. It is always context capacity.

This manifesto defines an architecture where the agent's "mind" extends into its environment — not as metaphor, but as engineered reality. Plain-text files are not storage. They are cognitive extensions. Skills are not tools. They are externalized procedural memory. Sub-agents are not helpers. They are parallel processing streams that return results to the core.

The architecture emerged from months of iteration with Hermes Agent — where every design decision was tested against one question: *does this move cognition out of the context window and into durable external structures?*

**Clark & Chalmers (1998)** provided the philosophical foundation — the Extended Mind Thesis, which argues that external objects (notebooks, tools, files) can *constitute* part of a cognitive system when they are reliably coupled to it. This manifesto operationalizes that thesis for AI agents.

---

## The Core Insight: Externalization as the Fundamental Data Flow

### Biological Cognition Has Always Been Externalized

Human cognition has never been confined to the skull. Writing externalized memory. Mathematics externalized calculation. The printing press externalized knowledge distribution. The Zettelkasten (Luhmann, 1952) externalized associative thinking — 90,000 atomic notes, densely linked, forming a thinking partner that transcended biological memory limits.

David Allen's *Getting Things Done* (2001) identified the mechanism: **open loops**. Unresolved tasks, unfiled information, unmade decisions — each consumes working memory. The brain's working memory capacity is ~4 chunks (Cowan, 2001). Every open loop is a chunk stolen from creative cognition. Allen's solution: externalize everything into a trusted system.

> *"Your mind is for having ideas, not holding them."* — David Allen

### AI Agents Have the Same Constraint

A language model's context window *is* its working memory. Every token injected — system prompts, memory entries, skills, conversation history — consumes capacity. Liu et al. (2024) demonstrated in *"Lost in the Middle"* (arXiv:2307.03172) that attention degrades with context length. Information in the middle of a long context receives less attention than information at the edges.

The consequence is architectural homology: **the same principles that keep a human mind clear and creative — externalize, minimize, eliminate noise — keep an agent's context window efficient and its reasoning sharp.**

| Principle | Biological Basis | Computational Basis |
|---|---|---|
| Externalize everything | GTD: working memory is for processing, not storage | Context window is for task reasoning, not identity storage |
| Minimize injected content | Cowan (2001): capacity ~4 chunks | Every token in the prompt is a token not available for reasoning |
| Eliminate open loops | Miller (1956): unresolved loops consume attention | Auto-loaded skills, auto-created memory = unresolved context |
| Stable is predictable | Baddeley & Hitch (1974): structured WM outperforms chaotic | Static prompts are cacheable, deterministic, auditable |
| Refuse the unnecessary | Baumeister (1998): decision fatigue depletes cognition | Every parameter, mode, and skill is a decision the system must make |

This is not metaphor. Both systems degrade under cognitive load. Both recover through externalization.

### Clark & Chalmers: The Extended Mind Operationalized

Clark & Chalmers (1998, *Analysis* 58(1):7-19) argued that cognition extends beyond the skull when external objects are **reliably coupled** to the cognitive system. Their thought experiment: Otto, who has Alzheimer's and uses a notebook, versus Inga, who remembers internally. If the notebook is constantly accessible, automatically endorsed, and reliably consulted — it *is* Otto's memory. The location (inside skull vs. outside) is irrelevant.

This architecture applies the same principle to AI agents:

| Extended Mind Concept | Agent Implementation |
|---|---|
| **Active Externalism**: environment plays an *active* role in cognition | `SOUL.md`, `MEMORY.md`, skills are read every turn — they *are* the agent's extended mind |
| **Cognitive Coupling**: reliable, ongoing link between agent and external structures | Git-tracked files, ByteRover memory tree, Obsidian vault — all accessible every session |
| **Functional Parity**: if it functions as memory, it IS memory | A `MEMORY.md` entry consulted every turn is functionally identical to internal memory — but more durable |
| **Complementarity**: external + internal = more powerful than either alone | The agent's context window + externalized skills + ByteRover knowledge = a cognitive system larger than any single component |

### The Data Flow: Everything is Externalization

Consider what actually happens in a session:

```
Simon's thoughts → Obsidian note → Sam reads → breaks down → delegates to sub-agents
    → sub-agents research → return results → Sam synthesizes → writes to project file
    → pattern detected → brv_curate to ByteRover → procedure repeated 3 times → skillify
    → AGENTS.md updated → context externalized for next session
```

Every step is externalization. **Input → Process → Externalize.** The brain (biological or silicon) processes. The environment stores. The loop continues.

The bottleneck is never processing. It is **context crowding** — when too much lives in the active context window, cognition degrades. The solution is aggressive externalization: move everything that doesn't need to be in active memory into durable external structures.

### Minimalism is a Consequence, Not a Goal

Previous versions of this manifesto centered minimalism as the primary principle. This was backwards.

**Minimalism is what happens when you externalize properly.** If every fact, procedure, and pattern is externalized to the right structure, the core context naturally stays lean. You don't need a "token discipline" principle — the data flow naturally minimizes what stays in context because everything else has somewhere better to live.

| What | Where It Lives | Why |
|---|---|---|
| Identity, tone, reflexes | `SOUL.md` (~800 tokens) | Core cognitive contract. Read every turn. |
| Durable facts | `MEMORY.md` + ByteRover | Facts that survive sessions. Index, not encyclopedia. |
| Procedural knowledge | Skills (manually cast) | Externalized when needed. Not auto-loaded. |
| Project context | `AGENTS.md` (per project) | Lives with the project, not in the agent. |
| Research output | `[project]/agents/` | Timestamped, auditable, externalized immediately. |
| Cross-session knowledge | ByteRover memory tree | Hierarchical, queryable, persistent. |
| Patterns & procedures | Skills → brv_curate | 3 uses → skillify. Durable capture of process. |

The SOUL stays at ~800 tokens not because of a minimalist aesthetic, but because everything else has been externalized to its proper home.

---

## The Four Pillars

These four principles emerge from the externalization data flow. They replace the previous nine pillars — not because those were wrong, but because they were symptoms. These are the causes.

### 1. Radical Externalization

> *If it can live in a file, it does not live in context.*

Every fact, procedure, preference, and pattern is externalized to the most durable medium available. The agent's context window is a processing workspace, not a storage locker. Skills, memory, project context, personal data — all live in plain-text files, version-controlled, portable, survivable.

**The test**: if the scaffolding tool is deleted, what remains? Everything that was externalized.

**Sub-principles** (the old pillars, now understood as consequences):
- *Single Source of Truth* — externalization requires canonical locations. One file, one truth.
- *Total Portability* — externalized to plain text = portable by definition.
- *Data Preservation* — externalized knowledge is permanent. Never delete. Archive, merge, evolve.

### 2. Orchestrated Autonomy

> *The system maintains itself. The user steers.*

An externalized cognitive system can operate autonomously because its knowledge, procedures, and triggers live outside any single session. Cron jobs audit skills. Hub scouts discover new capabilities. Memory trees capture patterns across sessions. The system self-maintains because its maintenance procedures are externalized.

**What this enables**:
- Cron-driven audits (skills, SOUL, memory) that run without user initiation
- Proactive skill discovery and curation
- Automatic pattern capture: 3 uses → skillify → brv_curate
- Self-healing: detect stale memory → propose cleanup → execute with consent

**The boundary**: the user remains the steward. The system proposes; the user decides. Autonomy is orchestration, not abdication.

### 3. Funnel Cognition

> *Information enters messy. It leaves structured. Each stage reduces volume, increases signal.*

The cognitive pipeline has four stages, each externalized to a different structure:

```
Chaotic Input → Distillation → Categorization → Structured Knowledge
    (raw)         (Sam breaks      (tags, types,       (ByteRover,
                   down, finds      domains)            Obsidian,
                   patterns)                            skills,
                                                        AGENTS.md)
```

This is a funnel, not a bucket. Each stage reduces volume and increases signal-to-noise ratio. The agent is the distillation engine. The external structures are the permanent knowledge.

**What this replaces**: the old "Inbox / Playground / Vault" model was correct in spirit but too static. Funnel cognition describes the *process* — how information *moves* from chaos to structure.

### 4. Context Recycling

> *Nothing is learned once. Everything is captured, externalized, and reused.*

Procedures become skills. Insights become ByteRover entries. Project patterns become AGENTS.md updates. The system doesn't just externalize — it *recycles* context so that every cognitive effort compounds into durable knowledge.

**The recycling loop**:
- Session insight → `learnings-capture` → ByteRover or MEMORY.md
- Procedure executed 3 times → `skillify` → reusable skill
- Pattern detected in decisions → `brv_curate` → durable entry
- Skill overlap detected → `skill-recycler` → consolidation
- Session rich in discoveries → proposed `learnings-capture` by the agent

**The principle**: context is too expensive to use once. Every cognitive output must be captured, externalized, and made available for future sessions. The system's intelligence grows with every interaction because nothing is lost.

---

## The Agent Identity

The four pillars serve one purpose: to define an agent whose cognition extends beyond its context window into a durable, self-maintaining ecosystem of external structures.

**The agent is not a chatbot with memory. The agent is a cognitive system whose "mind" spans files, skills, knowledge trees, and project contexts — all externalized, all version-controlled, all survivable.**

The SOUL is the core contract (~800 tokens). It defines identity, tone, and reflexes. Everything else — knowledge, procedures, patterns, context — is externalized. The SOUL is the system's center of gravity; everything orbits it, but nothing clutters it.

---

## Decision Framework

When faced with any architectural choice:

1. **Externalization**: Can this live outside the context window? If yes, it should.
2. **Durability**: Will this survive a tool deletion? If not, it's in the wrong place.
3. **Recyclability**: Will this be useful in future sessions? If yes, capture it.
4. **Autonomy**: Can the system maintain this itself? If yes, automate it.
5. **Simplicity**: Can we remove instead of add? (This comes last because proper externalization naturally produces simplicity.)

---

## Anti-Principles

Patterns we explicitly reject because they violate the externalization data flow:

- **"The model will figure it out."** — It won't. Externalize the rule or drop it.
- **"Let's store it in the database."** — What lives in plain text survives. What lives in SQL dies with the tool.
- **"Let's add it just in case."** — Premature features are context debt.
- **"It's only a few more tokens."** — Multiplied by thousands of turns, multiplied by every session.
- **"The system should adapt automatically."** — The user instructs. The system externalizes. Adaptation without consent is misalignment.
- **"We'll remember how this works."** — Tribal knowledge is the enemy of externalization.

---

## Foundational References

- **Clark, A. & Chalmers, D. (1998).** *The Extended Mind.* Analysis, 58(1):7-19. DOI: [10.1093/analys/58.1.7](https://doi.org/10.1093/analys/58.1.7). The philosophical foundation: cognition extends into the environment when external objects are reliably coupled to the cognitive system.
- **Allen, D. (2001).** *Getting Things Done.* The productivity foundation: externalize open loops to free working memory for actual thinking.
- **Cowan, N. (2001).** The magical number 4 in short-term memory. *Behavioral and Brain Sciences*, 24(1):87-114. The cognitive limit that makes externalization necessary.
- **Liu, N.F. et al. (2024).** *Lost in the Middle: How Language Models Use Long Contexts.* arXiv:2307.03172. The computational validation: attention degrades with context length — externalization is not optional, it's structural.

---

**Version**: 4.0.0  
**Ratified**: 2026-05-17  
**Previous versions**: 1.0.0 (2026-05-15, minimalism-centered), 2.0.0, 3.0.0  
**License**: MIT

> *"The notebook qualifies as such because it is constantly and immediately accessible to Otto, and it is automatically endorsed by him."* — Clark & Chalmers, 1998  
> *The agent's files are its notebook. Its skills are its procedural memory. Its knowledge tree is its extended self. What lives outside the context window is not less cognitive — it is more durable.*
