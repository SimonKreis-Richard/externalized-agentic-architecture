# Pattern: Funnel Cognition

> Ideas enter structured. Through capture, maturation, and harvest, they become actionable knowledge.

---

## Problem

In agentic systems, idea generation outpaces execution. Ideas arrive continuously (~10-15 per day) but implementation capacity is limited (~3-5 per day). Without a structured process:
- 70% of ideas are lost to context overflow
- Good ideas compete with urgent tasks for attention
- No mechanism for ideas to mature before implementation

## Solution

A three-stage funnel:

### Stage 1: Capture (The Goulotte)

**When:** Immediately. As ideas emerge.
**Where:** Structured backlog file.
**Rule:** If the idea survives 3 minutes of reflection, it merits an entry. No quality judgment at this stage.

### Stage 2: Store (The Silo)

Ideas mature in the backlog without pressure.

| Section | Content |
|---------|---------|
| **Pending** | Fresh ideas, unsorted |
| **Ripe** | Survived maturation + have clear implementation path |
| **Discarded** | Abandoned + reason (trace, not regret) |

### Stage 3: Harvest (The Scheduler)

Ripe ideas are extracted and implemented through:
- Dedicated sessions (periodic review)
- Automated scanning (cron-based proposals)
- Contextual triggers (session touches related domain → suggest related pending idea)

## Validation

- **Observed:** Without capture, 70% of ideas lost. With capture, 90% survive. With harvest, 40% implemented.
- **Extended Mind (Clark & Chalmers, 1998):** The funnel externalizes biological idea processing
- **MVP Methodology (Ries, 2011):** Capture raw, refine later — don't perfect before storing

## Anti-Patterns

- **Capturing without harvesting:** A funnel without output is hoarding
- **Forcing execution:** Ideas mature at their own pace
- **Over-categorizing:** Three stages (pending/ripe/discarded) is sufficient

---

## Sources

| Source | Relevance |
|--------|-----------|
| Clark & Chalmers (1998) | Externalized cognitive processes |
| Ries (2011) | MVP — capture raw, refine later |
| Observed data | Capture rates and survival statistics |
