# Principle: Progressive Disclosure

> Load depth on demand. Start shallow; go deep only when the task asks for it.
> **Family:** Context discipline · **Status in 2026:** partly platform-native (see below)

---

## Problem

An agent faces a loading dilemma with no comfortable default:

- Load everything up front → the context window fills with material that may
  never be used, and the relevant tokens compete for a shrinking attention
  budget.
- Load nothing → the agent lacks the procedures and knowledge it needs.
- Load on the first plausible match → it may pull the wrong thing and pay for it
  in tokens and confusion.

The cost is real: as context grows, recall and long-range reasoning degrade.
Material that is *present but unused* is not free — it dilutes attention.

## Principle

Expose knowledge in layers, and let the agent descend only as far as the task
requires:

| Layer | Content | Loaded |
|-------|---------|--------|
| **L0 — Catalog** | Names + one-line descriptions of available skills/docs | Always (cheap, in the system prompt) |
| **L1 — Body** | The full content of a skill or document | On trigger match |
| **L2 — References** | Specific sub-files a skill points to | On explicit need |

The agent scans L0, recognizes a match, pulls L1, and only follows L1's pointers
into L2 when the work actually reaches that depth. The catalog stays small; the
depth stays available.

## Theoretical grounding

- **Validated by** — Anthropic, *Effective context engineering* (2025): agents
  using "just-in-time" retrieval keep "lightweight identifiers (file paths,
  stored queries, web links)" and "assemble understanding layer by layer,
  maintaining only what's necessary in working memory." Progressive disclosure
  is named explicitly as an emergent benefit of this approach.
- **Validated by** — Liu et al., *Lost in the Middle* (2023): LLMs show a
  U-shaped attention curve — strongest at the start and end of context, weakest
  in the middle. Loading less, later, keeps high-value tokens out of the dead
  zone.
- **Consistent with** — Clark & Chalmers, *The Extended Mind* (1998): humans do
  not memorize corpora; we build external indexes (file systems, inboxes,
  bookmarks) and retrieve on demand. The catalog/body/reference split is that
  habit, externalized. *(Analogy, not proof.)*

## Practical application (DIY: Hermes / OpenClaw / custom harness)

1. **Build the L0 catalog.** Give every skill YAML frontmatter with a `name`,
   a tight `description`, and `triggers` (intent keywords). Concatenate just the
   names + descriptions into the system prompt. Keep the whole catalog under
   ~3K tokens; if it grows past that, you have a tagging problem, not a budget
   problem.
2. **Match, don't pre-judge.** On each task, scan the catalog for trigger /
   description overlap and load the matching body (L1). Do not ask the model to
   decide in advance whether it will "need" a skill — match mechanically, then
   let it read.
3. **Defer L2.** A skill body should *link* to heavy references (long tables,
   schemas, examples) rather than inline them. Load those only when the agent
   follows the link.
4. **Instrument it.** Log which skills load per session. Skills that never fire
   have bad triggers; skills that always fire probably belong in the core.

## User-space vs. platform-space

**Largely platform-native now.** The portable [Agent Skills format](https://www.claude.com/skills)
implements exactly this catalog → body → reference disclosure, and harnesses
load skills on demand without you wiring the trigger scan. If you build on a
modern platform, you mostly *author* skills rather than *implement* disclosure.

**Still yours if you DIY.** On a custom Hermes/OpenClaw setup you own the catalog
assembly, the trigger-matching pass, and the L2 deferral. This principle is the
blueprint for that layer.

## Anti-patterns

- **Eager loading** — pulling full skill bodies "just in case." Defeats the point.
- **Catalog bloat** — an L0 catalog so large it becomes the very context cost it was meant to avoid.
- **Missing triggers** — a skill with no intent keywords is invisible at L0; it will never be discovered.
- **Skipping layers** — jumping to an L2 reference without its L1 body, leaving the agent with detail and no frame.

## Sources

| Source | Label | Link |
|--------|-------|------|
| Anthropic — Effective context engineering for AI agents (2025) | Validated by | https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents |
| Liu et al. — Lost in the Middle (2023) | Validated by | https://arxiv.org/abs/2307.03172 |
| Clark & Chalmers — The Extended Mind (1998) | Consistent with | https://doi.org/10.1093/analys/58.1.7 |
| Anthropic — Agent Skills | Platform reference | https://www.claude.com/skills |
