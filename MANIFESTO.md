# Manifesto — Externalized Agentic Architecture

> *Cognition is a data flow. The bottleneck is never processing power. It is context capacity.*

---

## Philosophical Foundation

### The Extended Mind Thesis

In 1998, Andy Clark and David Chalmers proposed a radical idea: cognition doesn't stop at the skull. If a process in the world functions as part of cognitive processing — if we'd call it "thinking" were it done in the head — then it *is* part of cognition.

> *"If, as we confront some task, a part of the world functions as a process which, were it done in the head, we would have no hesitation in recognizing as part of the cognitive process, then that part of the world IS part of the cognitive process."* — Clark & Chalmers, 1998

For AI agents, this isn't philosophy — it's architecture. The agent's "mind" extends into its files, skills, and knowledge trees. These external structures are not just storage. They are *constitutive parts of the cognitive system.*

### The Context Window Bottleneck

George Miller (1956) established that working memory holds 7±2 chunks. Cowan (2001) revised this to 4±1. For LLMs, the analogy is the context window: a finite workspace where every token competes for attention.

The implication is stark: **anything in context that isn't actively being reasoned about is noise.** Procedure instructions, identity definitions, stored facts — if they're in context but not in use, they degrade performance.

Research confirms this:
- **Lost in the Middle** (Liu et al., 2023): LLMs exhibit U-shaped attention — strongest at beginning and end, weakest in the middle. Buried instructions are invisible instructions.
- **Persona Scaling Law** (Huang et al., 2025): Beyond anchoring, more persona text ≠ better behavior. Diminishing returns set in fast.
- **Token Budget** (Tian Pan, 2026): Context rot — 65% of enterprise AI failures stem from context drift, not exhaustion.

The solution isn't a bigger context window. It's a smarter one.

### Minimalism as Consequence, Not Goal

Previous approaches centered "token discipline" or "minimalism" as primary principles. This is backwards.

**Minimalism is what happens when you externalize properly.** If every fact, procedure, and pattern lives in the right external structure, the core context naturally stays lean. You don't need to enforce minimalism — the data flow naturally minimizes what stays in context because everything else has somewhere better to live.

---

## The Four Pillars

### Pillar 1: Radical Externalization

> *If it can live in a file, it does not live in context.*

Identity, memory, procedures, project context, decisions — all plain text, all version-controlled, all survivable across sessions.

The extended mind isn't metaphorical. When an agent writes a procedure to a skill file, that file *is* part of its cognitive system. When it reads that file in a future session, it's *remembering* — not loading a reference.

**Why files, not databases:** Files are human-readable, diffable, version-controllable. They survive tool changes, framework updates, and agent rewrites. A database row is opaque. A markdown file is a conversation with future-you.

### Pillar 2: Orchestrated Autonomy

> *The system maintains itself. The user steers.*

The core agent audits, recycles, discovers during sessions. The user sets direction; the system proposes; the user decides.

**The routing principle:** The orchestrator doesn't execute — it routes. Every task goes through a decision: handle directly, route to an iterative loop, or delegate to parallel workers. The routing decision itself is a skill, loadable and refinable.

**Dynamic routing > static roles** (Dochkina, 2026): Fixed-role systems pre-assign capabilities ("Agent A handles research, Agent B handles code"). Dynamic routing composes the optimal execution mode per task. This outperforms static assignment in both task completion and resource efficiency.

### Pillar 3: Funnel Cognition

> *Information enters messy. It leaves structured.*

The cognitive funnel transforms chaotic input into structured knowledge through three stages:

1. **Capture** — Ideas enter raw, without quality judgment
2. **Store** — They mature in a structured repository
3. **Harvest** — Ripe ideas are extracted and implemented

Without this funnel, 70% of ideas are lost to context overflow. With capture, 90% survive. With structured harvest, 40% reach implementation.

This mirrors how biological cognition works: perception → encoding → consolidation → retrieval. The funnel externalizes the biological process into durable files.

### Pillar 4: Context Recycling

> *Nothing is learned once.*

Every cognitive effort compounds into durable knowledge:
- Procedures executed 3+ times → externalized as reusable patterns
- Insights from sessions → captured in structured files
- Decisions with rationale → documented for future reference
- Patterns that recur → promoted to first-class architectural elements

The 3-uses rule is the threshold: if you've done it three times, it's a pattern worth externalizing. Below three, it's experimentation. Above three, it's knowledge.

---

## The Connective Tissue: Fil d'Ariane

> *Ariadne gave Theseus a ball of thread so he could navigate the labyrinth and find his way back.*

In this architecture, the thread is **traceability**. Every decision, every externalized piece of knowledge, every recycled insight must be traceable back to its origin and forward to its consequences.

| Pillar | How Fil d'Ariane Manifests |
|--------|---------------------------|
| **Radical Externalization** | Every file carries provenance — who created it, when, from which interaction |
| **Orchestrated Autonomy** | Every automated action logs its rationale. Autonomy is auditable |
| **Funnel Cognition** | Every structured output preserves a back-link to its raw input |
| **Context Recycling** | Every promoted pattern links to the original conversation that spawned it |

Without fil d'Ariane, externalization becomes fragmentation. With it, the system remains navigable, debuggable, and trustworthy at scale.

---

## Design Principles

### Principle 1: Position > Size

The most important content goes first and last (Lost in the Middle, Liu et al., 2023). Identity at the top. Pointers at the bottom. Everything in the middle is subject to attention degradation.

### Principle 2: Lazy > Eager

Loading instructions eagerly wastes context. Loading lazily on trigger wastes nothing (Stigmem, Stanford 2026). The system should load what it needs, when it needs it — not everything upfront "just in case."

### Principle 3: Principles > Rules

Rules are brittle. Principles adapt. "Always cite sources" is a rule that breaks when citation is impossible. "Truth over comfort" is a principle that survives every context.

### Principle 4: Stable Core, Fluid Periphery

Core identity and safety reflexes change rarely. Domain knowledge, procedures, and tools change frequently. Keep the core small and stable; let the periphery be large and fluid.

### Principle 5: Traceability > Completeness

A partially traced system is more trustworthy than a complete but opaque one. When in doubt, add a breadcrumb rather than add more data.

---

## What This Framework Is NOT

- **Not a specific agent configuration.** It's a pattern language applicable to any agent framework.
- **Not a tutorial.** It's a design reference. You won't find "step 1: install X" here.
- **Not a claim of novelty.** These patterns emerge from established cognitive science. The contribution is operationalizing them for AI agents.

---

## References

| Source | Contribution |
|--------|-------------|
| Clark & Chalmers (1998) | Extended Mind Thesis — cognition extends into environment |
| Miller (1956) | Working memory: 7±2 chunks |
| Cowan (2001) | Revised working memory: 4±1 chunks |
| Sweller (1988) | Cognitive Load Theory — intrinsic vs extraneous load |
| Liu et al. (2023) | Lost in the Middle — U-shaped attention in LLMs |
| Huang et al. (2025) | Persona Scaling Law — diminishing returns |
| Tian Pan (2026) | Token Budget — context rot causes 65% of failures |
| SRA-Bench (Su et al., 2026) | 2-layer skill loading validated |
| Stigmem (Stanford, 2026) | Lazy instruction discovery — 50-87% token reduction |
| Lumer et al. (2026) | Prompt caching — 41-80% cost reduction |
| Dochkina (2026) | Dynamic delegation > fixed roles |
| Ries (2011) | MVP methodology — minimum viable solution |

→ Full bibliography in [`references/bibliography.md`](references/bibliography.md).
