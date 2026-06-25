# Principle: Context Recycling

> Compact, distill, promote. Nothing worth knowing should be learned twice.
> **Family:** Context discipline · **Placement Rule:** Q3 · **2026:** part platform-native

---

## Problem

Over a long session, context accumulates: tool outputs, dead ends, resolved
sub-tasks. Left alone it pollutes the window. And across sessions, effort
evaporates — the same procedure gets re-derived because nobody captured it the
first three times.

## Principle

Two complementary moves:

- **Within a session — compact.** As the window fills, summarize and reinitialize:
  keep decisions, open problems, and key state; discard redundant tool output and
  noise.
- **Across sessions — promote.** When a procedure has been done enough times to
  be a pattern (a useful threshold: three), externalize it as a reusable skill.
  Below the threshold it's experimentation; above it, it's knowledge.

## Theoretical grounding

- **Validated by** — Anthropic, *Effective context engineering* (2025):
  compaction "distills the contents of a context window in a high-fidelity
  manner" — preserve architectural decisions and unresolved bugs, discard
  redundant tool results. Tool-result clearing is "one of the safest, lightest"
  forms.
- **Consistent with** — Hunt & Thomas, *The Pragmatic Programmer* (1999): DRY —
  every piece of knowledge should have one authoritative representation. The
  promote-to-skill move is DRY applied to agent procedures. *(Engineering
  heritage, not LLM evidence.)*
- **Engineering judgment** — the "three uses" threshold is a heuristic, not a
  measured constant. Treat it as a dial.

## Practical application (DIY)

- Configure a compaction trigger (token threshold) and a compaction prompt tuned
  for recall first, then precision. Clear stale tool results aggressively.
- Keep a lightweight running notes file the agent updates, so a compaction or a
  fresh session can rehydrate state.
- When you notice yourself re-typing the same guidance to the agent a third time,
  that's the signal: write the skill.

## User-space vs. platform-space

**Both.** Compaction is now a platform primitive (auto-compaction APIs); if your
harness has it, use it. Promotion-to-skill is yours: deciding *what* recurring
effort deserves to become durable knowledge is a judgment no API makes for you.

## Anti-patterns

- **Over-aggressive compaction** — summarizing away subtle context whose importance only surfaces later.
- **Capture without promotion** — notes that pile up and never become skills.
- **Premature promotion** — turning a one-off into a "pattern" before it has recurred.

## Sources

| Source | Label | Link |
|--------|-------|------|
| Anthropic — Effective context engineering (2025) | Validated by | https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents |
| Hunt & Thomas — The Pragmatic Programmer (1999) | Consistent with | https://pragprog.com/titles/tpp20/the-pragmatic-programmer-20th-anniversary-edition/ |
