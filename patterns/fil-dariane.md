# Pattern: Fil d'Ariane (Traceability)

> Every decision, every externalized knowledge, every recycled insight must be traceable back to its origin.

---

## Problem

Externalization creates fragments. Without traceability:
- Knowledge becomes orphaned (no one knows why it exists)
- Decisions become opaque (no rationale preserved)
- Debugging becomes impossible (can't trace errors to source)
- Trust degrades (can't verify claims back to evidence)

## Solution

Every artifact carries provenance:

| Artifact | Traceability Mechanism |
|----------|----------------------|
| **Files** | Creation timestamp, source session, author |
| **Decisions** | Rationale documented, alternatives considered |
| **Skills** | Origin conversation, 3-uses threshold met |
| **Patterns** | Original discovery, validation evidence |
| **Config changes** | Diff with rationale, blast radius documented |

The trace is bidirectional:
- **Forward:** "This decision led to these consequences"
- **Backward:** "This artifact was created because of this need"

## Validation

- **Ariadne Mythology:** The thread allows navigation of the labyrinth — here, the labyrinth is system complexity
- **Git Version Control:** Commits provide natural traceability for code and config
- **Audit Requirements:** Regulated industries require decision traceability

## Anti-Patterns

- **Orphan knowledge:** Files with no provenance — why does this exist?
- **Silent changes:** Modifying config without documenting rationale
- **Trace without access:** Storing traces in inaccessible locations

---

## Sources

| Source | Relevance |
|--------|-----------|
| Ariadne mythology | Foundational metaphor |
| Git SCM | Version control as traceability mechanism |
| ISO 27001 | Audit trail requirements |
