# Bibliography — Scientific Foundations

> Complete academic references supporting the Externalized Agentic Architecture framework.

---

## Foundational Cognitive Science

### Clark, A. & Chalmers, D. (1998)
**"The Extended Mind"**
*Analysis, 58*(1), 7-19. doi:10.1093/analys/58.1.7

**Key claim:** Cognition extends into the environment. When external processes function as cognitive processes, they *are* cognition.

**Relevance to this framework:** Foundational justification for externalizing identity, memory, and procedures into files. The agent's mind is not confined to the context window — it extends into its filesystem.

---

### Miller, G. A. (1956)
**"The Magical Number Seven, Plus or Minus Two: Some Limits on Our Capacity for Processing Information"**
*Psychological Review, 63*(2), 81-97.

**Key claim:** Working memory holds 7±2 chunks of information.

**Relevance:** Justifies hard caps on working memory and the need for externalization. Even generous context windows have attention limits.

---

### Cowan, N. (2001)
**"The Magical Number 4 in Short-Term Memory: A Reconsideration of Mental Storage Capacity"**
*Behavioral and Brain Sciences, 24*(1), 87-114.

**Key claim:** Working memory capacity is closer to 4±1 chunks.

**Relevance:** Strengthens the case for hard memory caps and lazy loading. The effective capacity is even smaller than Miller suggested.

---

### Sweller, J. (1988)
**"Cognitive Load During Problem Solving: Effects on Learning"**
*Cognitive Science, 12*(2), 257-285.

**Key claim:** Intrinsic cognitive load (task complexity) and extraneous cognitive load (presentation format) compete for working memory.

**Relevance:** Instruction-heavy system prompts create extraneous load that competes with task reasoning. The 3-tier reflex architecture minimizes extraneous load.

---

## LLM-Specific Research

### Liu, N. F. et al. (2023)
**"Lost in the Middle: How Language Models Use Long Contexts"**
*Transactions of the Association for Computational Linguistics (TACL)*. arXiv:2307.03172.

**Key claim:** LLMs exhibit U-shaped attention — strongest at beginning and end of context, weakest in the middle.

**Relevance:** Identity must be positioned first (primacy). Pointers to skills go last (recency). Middle content degrades. This drives the placement strategy for all system components.

---

### Huang, J. et al. (2025)
**"Persona Scaling Law: When Does Persona Prompting Work?"**
arXiv:2510.11734.

**Key claim:** Beyond anchoring, more persona text yields diminishing returns in behavior quality.

**Relevance:** Compact identity (~800-1200 tokens) outperforms verbose identity. The quality ceiling is reached quickly; additional text adds noise, not signal.

---

### Tian Pan (2026)
**"Token Budget Allocation for Agent Systems"**
April 2026.

**Key claim:** 4-tier allocation — static anchors (10-15%), retrieved context, conversation, scratch. Context rot causes 65% of enterprise AI failures.

**Relevance:** Drives the token budget model. Static anchors (identity + core skills) must be stable and compact. Dynamic content fills the rest.

---

## Agent Architecture Research

### Su et al. (2026) — SRA-Bench
**"SRA-Bench: Benchmarking Skill Retrieval and Augmentation for LLM Agents"**
arXiv 2604.24594, April 2026.

**Key claims:**
- 2-layer loading (always + retrieved) is optimal
- Passing the correct skill improves performance by 12-23 points
- 8 distractor skills drop accuracy from 60% to 45%
- This fragility does not disappear with larger models
- Beyond ~50 skills, retrieval is mandatory

**Relevance:** Validates the 2-layer skill architecture. Core skills (always loaded) + specialist skills (lazy). Also justifies keeping core skills few (3 is optimal).

---

### Stigmem — Stanford/Eidetic Labs (2026)
**"Lazy Instruction Discovery for LLM Agents"**
May 2026.

**Key claims:**
- Compact boot stub (<400 tokens) + recall on demand = 50-87% token reduction
- 0 regressions from lazy loading
- Intent-derived keywords beat content-derived keywords for retrieval triggers

**Relevance:** Validates progressive disclosure pattern. Compact identity + lazy skill loading is the optimal architecture. Also informs trigger design for skill discovery.

---

### Lumer et al. (2026)
**"Don't Break the Cache: Optimizing Prompt Caching for LLM Agents"**
January 2026. arXiv 2601.

**Key claims:**
- Prompt caching reduces API costs 41-80%
- TTFT improves 13-31%
- Static content first (system prompt + tools) = cacheable
- Dynamic content last = not cacheable

**Relevance:** Justifies the static prefix architecture. Identity + core skills must not change between turns to maximize cache hits.

---

### Dochkina (2026)
**"Dynamic Delegation in Multi-Agent Systems"**
Google Scaling Agent Study.

**Key claim:** Dynamic delegation (orchestrator composes optimal worker per task) outperforms static role assignment in task completion and resource efficiency.

**Relevance:** Validates the orchestrated routing pattern. The orchestrator routes, doesn't execute. Roles are composed per task, not pre-assigned.

---

### Do et al. (2024)
**"Dynamite Delegation: A Framework for Multi-Agent Orchestration"**
GAIA Benchmark.

**Key claim:** 38.7% improvement on GAIA benchmark with dynamic delegation framework.

**Relevance:** Empirical validation of dynamic orchestration over static multi-agent patterns.

---

## Design Methodology

### Ries, E. (2011)
**"The Lean Startup"**
*Crown Business.*

**Key claim:** Build-Measure-Learn. Minimum Viable Product (MVP) — ship the simplest version that works, then iterate.

**Relevance:** The MVP pattern in agent architecture — implement minimum viable behavior, test, iterate. Don't over-engineer before validating.

---

### Catrambone, R. (2002)
**"Problem Solving as an Organized Activity"**
*In: Handbook of Applied Cognition.*

**Key claim:** Structured procedures reduce cognitive load and improve transfer.

**Relevance:** Externalized procedures (skills) reduce agent cognitive load and improve consistency across sessions.

---

## Software Engineering

### Gamma, E. et al. (1994)
**"Design Patterns: Elements of Reusable Object-Oriented Software"**
*Addison-Wesley.*

**Key claim:** Documenting recurring design patterns improves software quality and maintainability.

**Relevance:** The patterns catalog in this framework follows the Gang-of-Four pattern format: Problem → Solution → Validation → Anti-patterns.

---

### Hunt, A. & Thomas, D. (1999)
**"The Pragmatic Programmer"**
*Addison-Wesley.*

**Key claim:** DRY (Don't Repeat Yourself). Every piece of knowledge should have a single, unambiguous representation.

**Relevance:** The 3-uses rule and context recycling pattern. Repeated procedures are promoted to single-source skills.

---

## Citation Format

When referencing this framework, use:

```
Externalized Agentic Architecture (2026). Design patterns for 
context-efficient AI agents. GitHub repository.
```

---

*Last updated: 2026-06-06*
