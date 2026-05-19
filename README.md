# Externalization Architecture for AI Agents

> **A principled framework for designing AI agents whose cognition extends beyond the context window into durable, self-maintaining external structures.**
>
> **Designed for Hermes Agent. Compatible with any scaffolding that reads plain-text files.**

---

## The Core Idea

**Cognition is a data flow.** Every thought — biological or artificial — is input → process → externalize. The bottleneck is never processing power. It is context capacity.

This framework operationalizes **Clark & Chalmers' Extended Mind Thesis (1998)** for AI agents: the agent's "mind" extends into its files, skills, and knowledge trees. These external structures are not just storage — they are *constitutive parts* of the cognitive system.

> *"If, as we confront some task, a part of the world functions as a process which, were it done in the head, we would have no hesitation in recognizing as part of the cognitive process, then that part of the world IS part of the cognitive process."* — Clark & Chalmers, 1998

---

## The Four Pillars

1. **Radical Externalization** — If it can live in a file, it does not live in context. Identity, memory, procedures, project context — all plain text, all version-controlled, all survivable.

2. **Orchestrated Autonomy** — The system maintains itself. Cron jobs audit, recycle, and discover. The user steers. The system proposes; the user decides.

3. **Funnel Cognition** — Information enters messy. It leaves structured. Chaotic Input → Distillation → Categorization → Structured Knowledge. Each stage reduces volume, increases signal.

4. **Context Recycling** — Nothing is learned once. Procedures → skills (3 uses rule). Insights → skills + memory system. Patterns → AGENTS.md. Every cognitive effort compounds into durable knowledge.

### The Golden Thread — Fil d'Ariane

> *Ariadne gave Theseus a ball of thread so he could navigate the labyrinth and find his way back. In this architecture, that thread is traceability.*

**Fil d'Ariane** (Ariadne's thread) weaves through all four pillars as the connective tissue of the system. It is the principle that every decision, every piece of externalized knowledge, every recycled insight must be traceable back to its origin — forward to its consequences.

| Pillar | How Fil d'Ariane Manifests |
|--------|---------------------------|
| **Radical Externalization** | Every file carries provenance — who created it, when, and from which interaction. No orphan knowledge. |
| **Orchestrated Autonomy** | Every automated action logs its rationale. Cron jobs leave breadcrumbs. Autonomy is auditable. |
| **Funnel Cognition** | Every structured output preserves a back-link to its raw input. The funnel is reversible for forensic analysis. |
| **Context Recycling** | Every promoted pattern or skill links to the original conversation or decision that spawned it. |

Without fil d'Ariane, externalization becomes fragmentation. With it, the system remains navigable, debuggable, and trustworthy at scale.

---

## Minimalism is a Consequence, Not a Goal

Previous versions centered minimalism as the primary principle. This was backwards. **Minimalism is what happens when you externalize properly.** If every fact, procedure, and pattern is externalized to the right structure, the core context naturally stays lean. You don't need "token discipline" — the data flow naturally minimizes what stays in context because everything else has somewhere better to live.

---

## What's in This Repo

| File | Purpose |
|------|---------|
| [`MANIFESTO.md`](MANIFESTO.md) | The constitution. Four pillars, foundational references, decision framework. |
| [`ARCHITECTURE.md`](ARCHITECTURE.md) | Technical implementation. Information flow, topology, memory architecture, component map. |
| [`SOUL-TEMPLATE.md`](SOUL-TEMPLATE.md) | Canonical SOUL template (~800 tokens). |
| [`skills-reference.md`](skills-reference.md) | Third-party skill install commands. |
| [`concepts/information-flow.md`](concepts/information-flow.md) | Deep dive: the data pipeline, 3-zone model, local-first paradigm. |
| [`concepts/example-agents.md`](concepts/example-agents.md) | Fictional AGENTS.md example. |
| [`concepts/fil-dariane.md`](concepts/fil-dariane.md) | Full specification of the traceability principle — provenance, audit trails, and forensic navigation. |
| [`concepts/architecture-overview.svg`](concepts/architecture-overview.svg) | System diagram: pillars, data flow, and component relationships at a glance. |
| [`config.yaml.template`](config.yaml.template) | Scaffold configuration template for quick setup. |

---

## Visual Overview

![Architecture Overview](concepts/architecture-overview.svg)

*System-level diagram showing the four pillars, information flow zones, and the fil d'Ariane traceability layer connecting all components.*

---

## Who This Is For

- **System architects** designing resilient, self-maintaining agent pipelines
- **Power users** who want an AI partner whose intelligence grows with every interaction
- **Platform engineers** building multi-agent orchestration layers
- **Hermes Agent users** — this architecture is built on Hermes
- **Anyone** who wants their AI to survive a tool change, machine wipe, or platform migration

---

## How It Works

1. **One SOUL.** `SOUL.md` (~800 tokens) defines identity, tone, and reflexes. Everything else is externalized.

2. **Everything is plain text.** Markdown, JSON, Git. Nothing proprietary. Nothing vendor-locked.

3. **Sub-agents are parallel processors.** Empty shells, explicit context, return results. They extend cognition, not personality.

4. **The system self-maintains.** Cron jobs audit, recycle, scout. Autonomy is orchestrated, not abdicated.

5. **Context recycles.** Procedures → skills. Insights → skills + memory system. Patterns → AGENTS.md. Nothing is lost.

6. **Every action leaves a thread.** The fil d'Ariane guarantees traceability — from raw input through every transformation to final output. Forensic replay, audit trails, and provenance queries are first-class capabilities.

---

## Architectural Principles for System Architects

| Principle | Rationale |
|-----------|-----------|
| **Fail-safe externalization** | If the agent crashes mid-task, no cognitive work is lost — it lives in files, not context. |
| **Deterministic replay** | Every externalized decision carries a breadcrumb trail. Reconstruct any past interaction from its trace. |
| **Graduated autonomy** | The system starts fully supervised. Autonomy is granted level by level as audit trails prove reliability. |
| **Locality of reference** | Related knowledge lives in adjacent files. No cross-repository hunts for connected context. |
| **Stateless runtime** | The agent process itself carries zero long-lived state. All state is in files. Restart without memory loss. |

---

## The Agent Identity

**The agent is not a chatbot with memory. The agent is a cognitive system whose "mind" spans files, skills, knowledge trees, and project contexts — all externalized, all version-controlled, all survivable.**

The identity is personal. The architecture is universal.

---

## Getting Started

```bash
# Clone the repository
git clone https://github.com/nousresearch/minimal-agentic-architecture.git
cd minimal-agentic-architecture

# Read the manifesto first — it explains the philosophy
cat MANIFESTO.md

# Then the architecture — it explains the mechanics
cat ARCHITECTURE.md

# Customize the SOUL template to define your agent's identity
cp SOUL-TEMPLATE.md SOUL.md

# Configure your scaffolding
cp config.yaml.template config.yaml
```

---

## Foundational Reference

**Clark, A. & Chalmers, D. (1998).** *The Extended Mind.* Analysis, 58(1):7-19. DOI: [10.1093/analys/58.1.7](https://doi.org/10.1093/analys/58.1.7)

---

**Version**: 5.0.0  
**Last updated**: 2026-05-19  
**License**: MIT
