# Externalization Architecture — A Manifesto

> *"Where does the mind stop and the rest of the world begin?"* — Clark & Chalmers, *The Extended Mind* (1998)

## What This Is

A design constitution for AI agent systems built on a single insight: **cognition is a data flow.** Every thought — biological or artificial — is input, processing, externalization. The bottleneck is never processing power. It is always context capacity.

This manifesto defines an architecture where the agent's "mind" extends into its environment — not as metaphor, but as engineered reality. Plain-text files are not storage. They are cognitive extensions. Skills are not tools. They are externalized procedural memory. Sub-agents are not helpers. They are parallel processing streams that return results to the core. The Git repository is not version control. It is the trace — the Ariadne's thread that allows any decision to be traced back to its origin.

This architecture operationalizes Clark & Chalmers's Extended Mind Thesis (1998) for AI agents: external objects (files, notes, skill directories) *constitute* part of a cognitive system when they are reliably coupled to it.

---

## The Core Insight: Externalization as the Complete Data Flow

### Biological Cognition Has Always Been Externalized

Human cognition has never been confined to the skull. Writing externalized memory. Mathematics externalized calculation. The printing press externalized knowledge distribution. The Zettelkasten (Luhmann, 1952) externalized associative thinking — 90,000 atomic notes, densely linked, forming a thinking partner that transcended biological memory limits.

David Allen's *Getting Things Done* (2001) identified the mechanism: **open loops**. Unresolved tasks, unfiled information, unmade decisions — each consumes working memory. The brain's working memory capacity is ~4 chunks (Cowan, 2001). Every open loop is a chunk stolen from creative cognition. Allen's solution: externalize everything into a trusted system.

> *"Your mind is for having ideas, not holding them."* — David Allen

### AI Agents Have the Same Constraint

A language model's context window *is* its working memory. Every token injected — system prompts, memory entries, skills, conversation history — consumes capacity. Liu et al. (2024) demonstrated in *"Lost in the Middle"* (arXiv:2307.03172) that attention degrades with context length. Information in the middle of a long context receives less attention than information at the edges.

Recent work by Su et al. (2026, arXiv:2604.24594) on **Skill Retrieval Augmentation (SRA-Bench)** confirms this empirically at scale: explicitly enumerating available skills within the context window fails to scale. As skill corpora expand, context budgets are consumed rapidly, and agents become markedly less accurate in identifying the right skill. The emerging paradigm — dynamic retrieval of skills from external corpora on demand — is not a convenience. It is a structural necessity.

The consequence is architectural homology: **the same principles that keep a human mind clear and creative — externalize, minimize, eliminate noise — keep an agent's context window efficient and its reasoning sharp.**

| Principle | Biological Basis | Computational Basis |
|---|---|---|
| Externalize everything | GTD: working memory is for processing, not storage | Context window is for task reasoning, not identity storage |
| Minimize injected content | Cowan (2001): capacity ~4 chunks | Every token in the prompt is a token not available for reasoning |
| Eliminate open loops | Miller (1956): unresolved loops consume attention | Auto-loaded skills, auto-created memory = unresolved context |
| Stable is predictable | Baddeley & Hitch (1974): structured WM outperforms chaotic | Static prompts are cacheable, deterministic, auditable |
| Load lazily, not eagerly | Stigmem (2026): boot stub + manifest beats full preload | Lazy instruction discovery reduces context by 50–87% |
| Refuse the unnecessary | Baumeister (1998): decision fatigue depletes cognition | Every parameter, mode, and skill is a decision the system must make |

This is not metaphor. Both systems degrade under cognitive load. Both recover through externalization.

### Clark & Chalmers: The Extended Mind Operationalized

Clark & Chalmers (1998, *Analysis* 58(1):7-19) argued that cognition extends beyond the skull when external objects are **reliably coupled** to the cognitive system. Their thought experiment: Otto, who has Alzheimer's and uses a notebook, versus Inga, who remembers internally. If the notebook is constantly accessible, automatically endorsed, and reliably consulted — it *is* Otto's memory. The location (inside skull vs. outside) is irrelevant.

This architecture applies the same principle to AI agents:

| Extended Mind Concept | Agent Implementation |
|---|---|
| **Active Externalism**: environment plays an *active* role in cognition | SOUL, MEMORY, skills are read every turn — they *are* the agent's extended mind |
| **Cognitive Coupling**: reliable, ongoing link between agent and external structures | Git-tracked files, skill system, Obsidian vault — all accessible every session |
| **Functional Parity**: if it functions as memory, it IS memory | A MEMORY.md entry consulted every turn is functionally identical to internal memory — but more durable |
| **Complementarity**: external + internal = more powerful than either alone | The agent's context window + externalized skills + memory system = a cognitive system larger than any single component |

---

## The Ariadne's Thread: Externalization as Trace

The central concept of this architecture is the **Ariadne's thread** — the trace that every cognitive act leaves in the external system. In the original myth, Theseus enters the labyrinth and unrolls a thread so he can retrace his path. Each decision, each procedure, each insight in this architecture produces a durable token that becomes part of the thread.

### The Complete Data Flow

The system is not merely "radically externalized" — it is a **pipeline** where every actor externalizes *to* the next actor, and the trace accumulates at every step:

```
Human thought → Note-taking system → Agent context → Skill system → Git repository
   (raw idea)     (structured note)   (task breakdown,    (procedural    (immutable
                                     delegation)          capture)       trace)
```

Each arrow is an act of externalization. Each externalization creates a token on the thread:

1. **Human → Note-taking system**: A thought is captured as a structured note (Obsidian, plain-text). This is externalization from biological to digital. The note becomes the first token on the thread.

2. **Note-taking system → Agent context**: The agent reads the note, decomposes the task, assigns sub-agents. The agent's reasoning is not ephemeral — it is recorded in conversation logs, agent files, and project documents. Each sub-agent result is externalized back to a file.

3. **Agent context → Skill system**: When a procedure is executed three times, it becomes a skill — externalized procedural memory. The act of skillification records the pattern so it never needs to be re-derived. This is the second token.

4. **Skill system → Git repository**: Every change — every file edit, every skill creation, every memory update — is committed to version control. Git is the master thread. Any decision in the system's history can be traced to the commit that introduced it, the conversation that motivated it, and the human thought that originated it.

```
Externalization Pipeline (complete):

┌────────────────┐     ┌────────────────┐     ┌────────────────┐     ┌────────────────┐
│  Human Thought  │ ──► │ Note-taking Sys │ ──► │  Agent Context  │ ──► │  Skill/Memory   │
│  (biological)   │     │  (Obsidian, md) │     │  (session)      │     │  (file system)  │
└────────────────┘     └────────────────┘     └────────────────┘     └────────────────┘
                                                                              │
                                                                              ▼
                                                                     ┌────────────────┐
                                                                     │  Git Repository │
                                                                     │  (the thread)   │
                                                                     └────────────────┘
```

**Every token in this pipeline is part of the thread.** The system is fully traceable: any decision, any procedure, any insight can be traced back to its origin by following the externalization chain in reverse (Git → skills → session logs → notes → human thought).

### The Thread is Not Metaphor

The Ariadne's thread is a concrete, verifiable property of the system:

- **Every Git commit** documents *why* a change was made, with references to the session that produced it.
- **Every skill** contains provenance metadata — which session created it, which pattern it encodes, when it was last validated.
- **Every memory entry** is timestamped and linked to the context that generated it.
- **Every sub-agent result** is saved to a project file before the session ends.

The thread is what makes the system **reversible**. If a procedure produces unexpected results, the thread allows the operator to walk back through each externalization step to find the point of divergence. Without the thread, the system is a black box that forgets its own history.

---

## The Four Pillars

These four principles emerge from the externalization data flow. They replace the previous nine pillars — not because those were wrong, but because they were symptoms. These are the causes.

### 1. Radical Externalization

> *If it can live in a file, it does not live in context.*

Every fact, procedure, preference, and pattern is externalized to the most durable medium available. The agent's context window is a processing workspace, not a storage locker. Skills, memory, project context, personal data — all live in plain-text files, version-controlled, portable, survivable.

**The test**: if the scaffolding tool is deleted, what remains? Everything that was externalized.

**Loaded lazily, never eagerly**: The Anthropic Agent Skills Architecture (2025) defines three levels of loading — metadata (~100 tokens, always loaded), instructions (~5k tokens, loaded on trigger), and resources (unlimited, loaded on demand). This architecture adopts the same principle. Skills are not auto-loaded into context. They exist as directories on the filesystem. The agent loads only the name and description at startup; the full instructions enter context only when the skill is triggered. Resources and scripts never enter context — they are executed via the filesystem, and only their output is consumed.

This mirrors the Stigmem "Stop Preloading Everything" finding (2026): replacing full instruction-file preloads with a boot stub (~300–500 tokens) plus an instruction manifest, backed by a recall tool, reduced per-heartbeat context consumption by 50–87% with zero regressions.

**Sub-principles** (the old pillars, now understood as consequences):
- *Single Source of Truth* — externalization requires canonical locations. One file, one truth.
- *Total Portability* — externalized to plain text = portable by definition.
- *Data Preservation* — externalized knowledge is permanent. Never delete. Archive, merge, evolve.

### 2. Orchestrated Autonomy

> *The system maintains itself. The user steers.*

An externalized cognitive system can operate autonomously because its knowledge, procedures, and triggers live outside any single session. Scheduled audits examine skills for staleness, detect dead references, and propose consolidation. Sub-agents are dispatched for parallel research and return structured results. Memory is captured across sessions and indexed for retrieval.

**What this enables**:
- Scheduled audits (skills, core identity, memory) that run without user initiation
- Proactive skill discovery and curation
- Automatic pattern capture: 3 uses → skillify → save as skill or memory
- Self-healing: detect stale memory → propose cleanup → execute with consent
- Parallel delegation: sub-agents research, synthesize, report — each result externalized to the thread

**The boundary**: the user remains the steward. The system proposes; the user decides. Autonomy is orchestration, not abdication.

### 3. Funnel Cognition

> *Information enters messy. It leaves structured. Each stage reduces volume, increases signal.*

The cognitive pipeline has four stages, each externalized to a different structure:

```
Chaotic Input → Distillation → Categorization → Structured Knowledge
    (raw)         (agent breaks      (tags, types,       (skills,
                   down, finds        domains)            Obsidian,
                   patterns)                              MEMORY, Git)
```

This is a funnel, not a bucket. Each stage reduces volume and increases signal-to-noise ratio. The agent is the distillation engine. The external structures are the permanent knowledge. The funnel is what produces the thread — each stage adds a token.

The SRA-Bench findings validate this architecture empirically: the bottleneck in skill augmentation is not retrieval but **incorporation** — the agent's ability to determine *which* skill to load and *when* external loading is actually needed. A funnel that gives the agent progressively structured access to external knowledge (rather than dumping everything into context) directly addresses this bottleneck.

### 4. Context Recycling

> *Nothing is learned once. Everything is captured, externalized, and reused.*

Procedures become skills. Insights become durable skill entries. Project patterns become project context updates. The system doesn't just externalize — it *recycles* context so that every cognitive effort compounds into durable knowledge.

**The recycling loop**:
- Session insight → captured → memory or skill
- Procedure executed 3 times → skillified → reusable skill
- Pattern detected in decisions → saved as skill or memory → durable entry
- Skill overlap detected → consolidation → unified skill
- Session rich in discoveries → proposed capture by the agent

**The principle**: context is too expensive to use once. Every cognitive output must be captured, externalized, and made available for future sessions. The system's intelligence grows with every interaction because nothing is lost. Each recycling step adds another token to the Ariadne's thread.

---

## The Agent Identity

The four pillars serve one purpose: to define an agent whose cognition extends beyond its context window into a durable, self-maintaining ecosystem of external structures.

**The agent is not a chatbot with memory. The agent is a cognitive system whose "mind" spans files, skills, knowledge trees, and project contexts — all externalized, all version-controlled, all survivable, all threaded by traceable externalization tokens.**

The core identity contract (~800 tokens) defines identity, tone, and reflexes. Everything else — knowledge, procedures, patterns, context — is externalized. The core contract is the system's center of gravity; everything orbits it, but nothing clutters it.

---

## Decision Framework

When faced with any architectural choice:

1. **Externalization**: Can this live outside the context window? If yes, it should.
2. **Thread traceability**: Does this decision produce a durable token on the thread? If not, how will future operators trace the origin of this choice?
3. **Durability**: Will this survive a tool deletion? If not, it's in the wrong place.
4. **Recyclability**: Will this be useful in future sessions? If yes, capture it.
5. **Lazy loading**: Does this need to be in context right now, or can it be loaded on demand? Prefer the latter.
6. **Autonomy**: Can the system maintain this itself? If yes, automate it.
7. **Simplicity**: Can we remove instead of add? (This comes last because proper externalization naturally produces simplicity.)

---

## Design Principles: Constraints That Shape the System

Beyond the four pillars, the architecture is shaped by explicit design constraints — principles that guide every decision about whether to add, remove, or change something.

### The Apple Principle — "Simplicity is a constraint, not a consequence"

> *"Simplicity is not a natural outcome of good design. It is a constraint imposed at every stage."*

The README states that "minimalism is what happens when you externalize properly." This is true — **if** you also impose simplicity as a hard constraint. Without the constraint, externalization can produce a sprawling ecosystem of skills, memory entries, knowledge trees, and project files — an externalized mess instead of an internal one.

The Apple Principle is named after the design philosophy that made Apple products distinctive: every feature, every pixel, every animation passes through the filter of *"Is this the simplest possible version of what we need?"* Not *"Can we add this?"* but *"Can we achieve the goal without adding this?"*

**Applied to this architecture:**

| Domain | With Simplicity as Consequence Only | With Simplicity as Constraint |
|--------|-----------------------------------|------------------------------|
| **Skills** | Create a skill for every procedure after 3 uses → 200+ skills, hard to find the right one | Create a skill only when the procedure is both (a) used 3+ times AND (b) genuinely non-obvious → 40 focused skills |
| **Memory** | Capture every insight in MEMORY.md → bloated index, noisy reads | Capture only insights that (a) change behavior (b) are non-obvious (c) survive next month → compact, high-signal |
| **SOUL** | Add every behavioral rule discovered → 2000-token SOUL that nobody reads | Each rule must pass: "Is this still true if I remove it?" → 800-token SOUL, every line proven essential |
| **Config** | Every optimization gets a YAML key → 600-line config that becomes its own project | Every YAML key must pay rent: prove it changes behavior in a meaningful way → 50 lines |

**The test**: For any existing element (skill, memory entry, config key, rule), ask: *"If this were removed, would the system break in a meaningful way?"* If the answer is no, the element fails the Apple Principle.

**The paradox**: Simplicity is harder than complexity. It requires more discipline, more judgment, more deletion. The architecture makes it *possible* to achieve simplicity (through proper externalization). The Apple Principle makes it *inevitable* (through relentless constraint).

### The Durability Imperative

> *"If it can survive a tool swap, it should. If it can't, it shouldn't exist."*

Every element in the system is measured against one question: *"What happens if Hermes Agent is deleted right now?"*

- Plain-text files → still there
- Git history → still there
- Skills as files → still there
- Memory system entries → gone (they live in the scaffolding's state)
- Knowledge tree in the scaffolding → gone

The imperative: move everything that can be plain-text into plain-text. Know exactly what you lose with the scaffolding and what survives.

### The Zero-Overhead Documentation Rule

> *"If it matters, write it down. If it doesn't, delete it."*

Documentation with overhead (separate tools, special formats, extra processes) doesn't last. The only documentation that survives is documentation that costs nothing to write and nothing to maintain — which means it's already part of the system's natural output (commits, file headers, frontmatter metadata).

---

## Anti-Principles

Patterns we explicitly reject because they violate the externalization data flow and break the thread:

- **"The model will figure it out."** — It won't. Externalize the rule or drop it.
- **"Let's store it in the database."** — What lives in plain text survives. What lives in SQL dies with the tool.
- **"Let's add it just in case."** — Premature features are context debt.
- **"It's only a few more tokens."** — Multiplied by thousands of turns, multiplied by every session.
- **"The system should adapt automatically."** — The user instructs. The system externalizes. Adaptation without consent is misalignment.
- **"We'll remember how this works."** — Tribal knowledge is the enemy of externalization.
- **"Load everything at startup."** — Eager loading crowds the context window. Load lazily or not at all.
- **"This was decided last session, we don't need to document it."** — If it isn't on the thread, it never happened.
- **"The Sunk Cost Fallacy" — 3 failures = STOP. Root cause, not patch.** — Three failures on the same task is never a coincidence. The approach is wrong. The framing is wrong. The tool is wrong. Continuing down the same path because you have already invested tokens is the single most expensive cognitive error in an agentic system — it compounds wasted reasoning into wasted sessions. STOP. Analyze root cause. Change approach. Do not patch a fundamentally broken strategy.

---

## Foundational References

- **Clark, A. & Chalmers, D. (1998).** *The Extended Mind.* Analysis, 58(1):7-19. DOI: [10.1093/analys/58.1.7](https://doi.org/10.1093/analys/58.1.7). The philosophical foundation: cognition extends into the environment when external objects are reliably coupled to the cognitive system.
- **Allen, D. (2001).** *Getting Things Done.* The productivity foundation: externalize open loops to free working memory for actual thinking.
- **Cowan, N. (2001).** The magical number 4 in short-term memory. *Behavioral and Brain Sciences*, 24(1):87-114. The cognitive limit that makes externalization necessary.
- **Liu, N.F. et al. (2024).** *Lost in the Middle: How Language Models Use Long Contexts.* arXiv:2307.03172. The computational validation: attention degrades with context length — externalization is not optional, it's structural.
- **Su, W. et al. (2026).** *Skill Retrieval Augmentation for Agentic AI (SRA-Bench).* arXiv:2604.24594. DOI: [10.48550/arXiv.2604.24594](https://doi.org/10.48550/arXiv.2604.24594). Empirically confirms that enumerating skills in context fails to scale; proposes dynamic retrieval + incorporation as the solution. Reveals that the bottleneck is not retrieval but the agent's ability to determine *when* to load a skill.
- **Offbyonce / Eidetic Labs (2026).** *Stigmem: Stop Preloading Everything — How We Cut AI Agent Context by 50–87% with Lazy Discovery.* [stigmem.dev](https://stigmem.dev) | [GitHub](https://github.com/Eidetic-Labs/stigmem). Proves that replacing full instruction-file preloads with a boot stub + manifest + recall tool reduces context consumption by 50–87% with zero regressions. The principle: load triggers should describe intents, not content.
- **Anthropic (2025).** *Agent Skills Architecture.* [docs.anthropic.com](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview). Defines the three-level progressive disclosure model for skills: Level 1 (metadata, ~100 tokens, always loaded), Level 2 (instructions, ~5k tokens, loaded on trigger), Level 3 (resources/code, unlimited, accessed on demand via filesystem). This architecture operationalizes the same principle.
- **Luhmann, N. (1952–1997).** *Zettelkasten.* The original externalized associative thinking system: 90,000 atomic notes forming a thinking partner. The precursor to modern agent memory architectures.

---

## Version History

| Version | Date | Changes |
|---|---|---|
| 5.0.0 | 2026-05-19 | Ariadne's thread concept; complete dataflow pipeline (human → notes → agent → skills → Git); lazy loading as first-class principle; added SRA-Bench, Stigmem, and Anthropic Agent Skills references; removed all personal identifiers; restructured around traceability |
| 4.0.0 | 2026-05-17 | Four pillars replacing nine; funnel cognition; context recycling; minimalism as consequence |
| 3.0.0 | — | Skill system, sub-agent architecture |
| 2.0.0 | — | Cross-session memory, knowledge trees |
| 1.0.0 | 2026-05-15 | Initial version, minimalism-centered |

---

> *"The notebook qualifies as such because it is constantly and immediately accessible to Otto, and it is automatically endorsed by him."* — Clark & Chalmers, 1998
>
> *The agent's files are its notebook. Its skills are its procedural memory. Its Git history is its Ariadne's thread — every externalization is a point of return. What lives outside the context window is not less cognitive — it is more durable, more traceable, and more reversible.*

**Version**: 5.0.0
**Ratified**: 2026-05-19
**License**: MIT
