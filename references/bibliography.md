# Bibliography

> Every source here has been opened and verified. Each carries one of three
> labels — **Validated by** (direct empirical/practitioner evidence on LLMs),
> **Consistent with** (a cognitive-science analogy that orients design, not a
> proof), or **Engineering judgment** (convergent practice, no specific paper).
> Nothing is dressed up as something it is not.

---

## Practitioner & empirical evidence on agents (Validated by)

### Anthropic — Effective context engineering for AI agents (2025)
https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
The anchor for this whole framework. Establishes context as a finite resource
and the "attention budget"; gives the guiding line — "the smallest possible set
of high-signal tokens that maximize the likelihood of some desired outcome";
documents just-in-time retrieval, compaction, structured note-taking, and
sub-agent architectures.

### Anthropic — Building effective agents (2024)
https://www.anthropic.com/research/building-effective-agents
Workflows vs. agents; start simple, add orchestration only when it helps.
Grounds *Orchestrated Routing*.

### Anthropic — How we built our multi-agent research system (2025)
https://www.anthropic.com/engineering/multi-agent-research-system
Sub-agents returning distilled summaries outperform single-agent systems on
complex research. Grounds *Dynamic Delegation*.

### Liu, N. F. et al. — Lost in the Middle (2023)
https://arxiv.org/abs/2307.03172
LLMs show U-shaped attention: strongest at the start and end of context, weakest
in the middle. Grounds the position arguments in *Pointer Identity* and
*Progressive Disclosure*.

### Lumer, E. et al. — Don't Break the Cache (2026)
https://arxiv.org/abs/2601.06007
Prompt caching cuts cost 41–80% and improves TTFT 13–31% when the static prefix
is kept stable. Supports keeping the core stable (*Pointer Identity*).

### Chroma — Context Rot (research)
https://research.trychroma.com/context-rot
Recall degrades as context length grows. The empirical basis for the
"finite resource with diminishing returns" claim.

## Cognitive-science framing (Consistent with — analogy, not proof)

### Clark, A. & Chalmers, D. — The Extended Mind (1998)
https://doi.org/10.1093/analys/58.1.7
External structures that function as cognition are part of the cognitive system.
The framing for *Externalized Memory* and *The Reflective Mirror*.

### Miller, G. A. — The Magical Number Seven (1956)
https://doi.org/10.1037/h0043158
Working memory ≈ 7±2 chunks. Motivates hard caps on always-loaded context.

### Cowan, N. — The Magical Number 4 (2001)
https://doi.org/10.1017/S0140525X01003922
A tighter estimate of working-memory capacity. Motivates the working-memory cap
in *Externalized Memory*.

### Sweller, J. — Cognitive Load During Problem Solving (1988)
https://doi.org/10.1207/s15516709cog1202_4
Intrinsic vs. extraneous load compete for working memory. Frames instruction-heavy
prompts as extraneous load.

## Engineering heritage (Consistent with / Engineering judgment)

### Hunt, A. & Thomas, D. — The Pragmatic Programmer (1999)
https://pragprog.com/titles/tpp20/the-pragmatic-programmer-20th-anniversary-edition/
DRY and rubber-duck debugging. Grounds *Context Recycling* and *The Reflective Mirror*.

### Gamma, E. et al. — Design Patterns (1994)
https://www.oreilly.com/library/view/design-patterns-elements/0201633612/
The problem → solution → consequences format this catalog borrows.

### Plato — Apology (Socratic method)
https://www.gutenberg.org/ebooks/1656
Understanding through articulation and questioning. Framing for *The Reflective Mirror*.

---

## Removed sources (transparency)

Earlier drafts of this repo cited the following, which **could not be verified
and have been removed**. We list them so the correction is on the record:

- *"Stigmem — Stanford/Eidetic Labs (2026)"* — no such paper or lab found. Fabricated.
- *"Dochkina (2026), Google Scaling Agent Study"* — not found. Fabricated.
- *"Do et al. (2024), Dynamite Delegation, 38.7% on GAIA"* — not found. Fabricated.
- *"Persona Scaling Law: When Does Persona Prompting Work? (Huang et al., 2025)"* —
  the arXiv ID 2510.11734 is real, but the paper is *"Scaling Law in LLM Simulated
  Personality: More Detailed and Realistic Persona Profile Is All You Need"* and
  argues roughly the **opposite** of what was claimed. Removed.
- *"SRA-Bench: Benchmarking… (Su et al., 2026)"* with specific stats — a real
  paper exists ("Skill Retrieval Augmentation for Agentic AI", arXiv 2604.24594)
  but the cited title and figures were not verifiable, so the citation was dropped
  rather than misrepresent it.
- *"Tian Pan (2026), Token Budget Allocation"* — a real blog post, not a paper,
  with an unverifiable headline statistic. Dropped; the point it supported is
  already covered by the Anthropic guide above.

*Last verified: 2026-06-24*
