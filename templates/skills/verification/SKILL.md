---
name: verification
description: >-
  Definitively verify subagent output, tool results, and external claims before
  reporting to the user. Detects hallucinations, requires verifiable handles,
  cross-references sources, and applies the 4-phase root cause investigation
  protocol. Load this whenever a subagent returns claims, writes files, makes
  HTTP calls, or reports success — or when debugging test failures, bugs,
  integration issues, or config problems. French trigger phrases: "vérifie",
  "debug", "vérification", "4 phases", "root cause".
domain: coding
category: coding
importance_score: 8
generalization_score: 9
author: '[USER_NAME] + [AGENT_NAME]'
source:
  - skills-curated/custom-core/verification-methodology/SKILL.md
  - skills-curated/built-in/systematic-debugging/SKILL.md
  - skills-curated/custom-core/config-verification-patterns/SKILL.md
merged_from:
  - verification-methodology
  - systematic-debugging
  - config-verification-patterns
fork_date: 2026-05-27
consumes_official: false
suggested_loading: always
core: true
keywords:
  - verification
  - hallucination detection
  - trust-but-verify
  - root cause analysis
  - debugging
  - config audit
  - cross-reference
  - verifiable handle
triggers:
  - "vérifie"
  - "debug"
  - "vérification"
  - "4 phases"
  - "root cause"
  - "verify"
  - "subagent output"
  - "config.yaml"
  - "change rien"
  - "audit"
  - "repo alignment"
  - "design principles coverage"
  - "est-ce que c'est bien présent"
  - "est-ce que c'est justifié"
---

# Verification — Trust But Verify, Debug by Root Cause

> Merges the SOUL verification reflex with the 4-phase debugging protocol and
> configuration integrity checks. Subagents self-report. Self-reports are
> unreliable. This skill defines HOW to verify — and HOW to investigate when
> things break.

---

## When to Load

Load this skill when ANY of the following apply:

**Verification triggers:**
- A subagent reports "file written", "uploaded", "PR created", or "deployed"
- A subagent makes a factual claim based on research
- You receive a subagent summary and need to confirm it before forwarding to the user
- You need to cross-reference sources for confidence
- You suspect a hallucination (confident lie, fabricated URL, over-generalization, silent failure)

**Debugging triggers:**
- A test fails, a build breaks, or unexpected behavior appears
- You're under time pressure and tempted to skip investigation
- You've already tried multiple fixes without resolution
- You don't fully understand why something is happening
- "Just one quick fix" seems obvious (it never is)

**Config verification triggers:**
- After changing `config.yaml` or `.env`
- After updating provider keys or MCP endpoints
- After modifying curator settings
- Before restarting the agent after config changes

---

## Overview

This skill unifies four verification traditions into one discipline:

1. **Trust-but-verify methodology** — The core insight: subagents are unreliable narrators. Every side-effect operation must produce a verifiable handle (file path, URL, HTTP status) that you check independently. Hallucination detection patterns catch confident lies, fabricated URLs, over-generalizations, and silent failures.

2. **4-phase root cause debugging** — NEVER fix a bug before you understand its root cause. Investigate (Phase 1), analyze patterns (Phase 2), form and test hypotheses (Phase 3), then implement the fix (Phase 4). Symptom fixes are failures. Three failed fixes means the architecture itself is wrong.

3. **Configuration verification patterns** — After config changes, run a 5-point checklist: YAML syntax, Jina MCP quote integrity, curator enabled flag, Gemini usage scope, and fallback model availability. Curator self-reports can hallucinate — always verify with `curator status`.

4. **Documentation gap analysis** — When auditing a documentation repo against design principles, check for 4 categories of systematic gaps: missing mechanism explanations, missing latency/performance arguments, missing portability/lock-in arguments, and missing economic/market context. See `references/documentation-gap-analysis.md`.

---

## Workflow

### Track 1: Verify a Subagent's Output

```
1. Subagent returns → scan for verifiable handles
   ├── File path?  → read_file(path) or stat(path)
   ├── URL?        → fetch or web_extract to confirm
   ├── ID/handle?  → query the system to confirm
   └── None?       → Task incomplete. Re-delegate with handle requirements.

2. For research claims → cross-reference 2+ independent sources
   ├── 1 source        → Low confidence, find more
   ├── 2 sources agree → Medium confidence, accept with caveat
   ├── 3+ agree        → High confidence, accept
   └── Disagree        → Investigate which is more authoritative

3. Check for hallucination patterns
   ├── Confident lie with no source       → discard, re-delegate
   ├── Plausible URL that 404s            → fabricated, re-delegate
   ├── Over-generalization from 1 example → require scope
   └── Silent failure (exit code ≠ 0)    → check raw output

4. For critical ops (deployments, migrations, security):
   └── Spawn a second subagent whose ONLY job is to verify the first
```

### Track 2: Debug a Bug (4 Phases)

```
PHASE 1 — Root Cause Investigation (NO FIXES YET)
├── Read error messages fully — stack traces, line numbers, error codes
├── Reproduce consistently — exact steps, every time?
├── Check recent changes — git log, git diff, new deps
├── For multi-component systems: add instrumentation at each boundary
├── Trace data flow — where does the bad value originate?
└── [x] Error understood, [x] reproduced, [x] changes reviewed,
    [x] evidence gathered, [x] root cause hypothesis formed

PHASE 2 — Pattern Analysis
├── Find working examples in the same codebase
├── Compare broken vs working — list every difference
├── Read reference implementations completely (no skimming)
└── Understand what assumptions the broken code makes

PHASE 3 — Hypothesis & Testing
├── Form one specific hypothesis: "X is the root cause because Y"
├── Make the SMALLEST possible change to test it
├── One variable at a time
└── Verify result → if passed, Phase 4; if failed, new hypothesis

PHASE 4 — Implementation
├── Create a failing test case that reproduces the bug (RED)
├── Implement ONE fix addressing the root cause
├── Verify: test passes, no regressions (GREEN)
├── If fix fails → count attempts
│   ├── < 3 → return to Phase 1 with new info
│   └── ≥ 3 → STOP. Question the architecture. Discuss with user.
└── No "while I'm here" improvements. No bundled refactoring.
```

### Track 3: Verify Configuration Changes

After any change to `config.yaml` or `.env`:

```
1. YAML syntax   → python3 -c "import yaml; yaml.safe_load(open('config.yaml'))"
2. Jina MCP      → Verify Authorization header has 2 quotes: '${JINA_API_KEY}'
3. Curator       → grep 'curator.enabled' — ensure it's 'true', not 'fasle'
4. Gemini usage  → Check only vision/extract/search use Gemini, not approval/mcp/triage
5. Fallback      → Ensure fallback_model is uncommented and points to a reliable model
6. .env          → Preserve comments (they're docs). Don't clean up unless asked.
7. Curator state → Run `curator status`, then `curator run`,
                    then `curator status` again to verify
```

---

## Tools & Commands

### Verification Tools

| Tool | What to Verify |
|------|---------------|
| `read_file` | File content, first/last lines, specific sections |
| `search_files` | File existence, content patterns, error strings |
| `terminal` + `stat`/`ls` | File existence, size, modification time |
| `terminal` + `curl -I` | URL existence, HTTP status |
| `web_extract` / `mcp_jina_ai_read_url` | Web content, page existence |
| `terminal` + `git log -1` | Recent commits, verify push |
| `terminal` + tests | Functional correctness |
| `mcp_tavily_tavily_extract` | Multi-page content extraction |
| `mcp_jina_ai_parallel_read_url` | Batch URL verification |

### Debugging Commands

```bash
# Reproduce a specific test failure
pytest tests/test_module.py::test_name -v --tb=long

# Check recent changes
git log --oneline -10
git diff
git log -p --follow src/problematic_file.py | head -100

# Trace function references
# (via search_files tool)
search_files("function_name(", path="src/", file_glob="*.py")
search_files("variable_name\\s*=", path="src/", file_glob="*.py")

# Run full test suite to check for regressions
pytest tests/ -q
```

### Config Verification Commands

```bash
# YAML syntax check
python3 -c "import yaml; yaml.safe_load(open('config.yaml'))"

# Curator check
curator status
curator run

# Gemini usage audit
grep -n 'gemini-3.1-flash-lite' config.yaml
```

### Delegating Investigation Subagents

For complex multi-component debugging:

```python
delegate_task(
    goal="Investigate why [specific test/behavior] fails",
    context="""
    Follow the verification skill's 4-phase debugging protocol:
    1. Read the error message carefully
    2. Reproduce the issue
    3. Trace the data flow to find root cause
    4. Report findings — do NOT fix yet
    After finishing, return a verifiable handle: the absolute file path
    to any logs or evidence you collected.

    Error: [paste full error]
    File: [path to failing code]
    Test command: [exact command]
    """,
    toolsets=['terminal', 'file']
)
```

---

## Pitfalls

1. **Believing subagents at their word.** A subagent that says "file written successfully" may have written to the wrong path, with wrong content, or failed silently. Always verify side-effects with `read_file` or `stat` before reporting success to the user.

2. **Fixing symptoms instead of root causes.** Skipping Phase 1 (root cause investigation) and jumping straight to fixes is the #1 source of wasted debugging time. The "obvious fix" is wrong more often than not. If you haven't traced the data flow, you don't know what's broken.

3. **Multiple simultaneous fixes.** Changing several things at once means you can't isolate which change fixed (or broke) anything. One variable at a time, always. No "while I'm here" improvements during a bug fix.

4. **Trusting a single source for research claims.** Two articles citing the same press release is one source, not two. Independence matters. Require 2+ genuinely independent sources before accepting a factual claim.

5. **Not asking for verifiable handles in delegation context.** If you don't ask for a file path, URL, or status code upfront, subagents will often return unverifiable summaries. Make verifiable handles a hard requirement in every delegation that has side-effects.

6. **Curator `fasle` typo.** A single character typo — `fasle` instead of `false` — silently disables the curator. Always grep for `curator.enabled` after config changes. The command `curator dry-run` does not exist; run `curator run` directly.

7. **The Rule of Three violation.** After 3 failed fix attempts, continuing to try more fixes is a trap. Three failures means the architecture is wrong, not the hypothesis. Stop and question fundamentals before trying fix #4.

8. **Cleaning .env comments.** Comments in `.env` are documentation that helps the user remember what variables are available. Don't clean them up unless explicitly asked — size is not a concern, and comments have no runtime impact.

9. **Verifying pure computation.** Don't waste tokens verifying math, sorting, formatting, or well-known facts. Verify side-effects, spot-check research, trust computation.

---

## Verification Checklist

- [ ] Subagent returned a verifiable handle (file path, URL, HTTP status) for every side-effect operation
- [ ] File writes confirmed via `read_file` or `stat` — correct path, correct content
- [ ] HTTP operations confirmed via GET/HEAD on the resource URL
- [ ] Research claims cross-referenced against 2+ independent sources
- [ ] Hallucination patterns checked: no confident lies, fabricated URLs, over-generalizations, or silent failures
- [ ] Root cause identified before any fix was attempted (Phase 1 complete)
- [ ] Bug reproduced consistently with exact steps
- [ ] Single fix applied, one variable at a time
- [ ] Regression test written before fix was applied (RED → GREEN)
- [ ] Full test suite passes with no regressions
- [ ] Config.yaml passes 5-point syntax check (YAML, Jina quotes, curator, Gemini scope, fallback)
- [ ] Curator state verified after run (`status` before and after)
- [ ] No "while I'm here" improvements bundled
- [ ] If ≥ 3 fixes failed: architecture questioned, user consulted

---

## References

- `references/documentation-gap-analysis.md` — 4-category gap analysis methodology for auditing documentation repos against design principles. Check: (1) missing mechanism explanations, (2) missing latency/performance arguments, (3) missing portability/lock-in arguments, (4) missing economic/market context.

- `skills-curated/custom-core/verification-methodology/SKILL.md`
- `skills-curated/built-in/systematic-debugging/SKILL.md`
- `skills-curated/custom-core/config-verification-patterns/SKILL.md`

---

## Merged Sources

| Source | What was kept | What was discarded |
|--------|--------------|-------------------|
| **verification-methodology** | Core verification protocol (tiers 1-3), hallucination detection patterns (4 patterns), verifiable handles contract, cross-referencing table, two-agent verification pattern, verification by tool table, "what NOT to verify" guidance | Duplicate integration notes with delegation (covered by workflow section) |
| **systematic-debugging** | 4-phase protocol (full workflow), the Iron Law, red flags/rationalizations table, investigation tools with integration, Rule of Three, architectural questioning on 3+ failures | Duplicate "when to use" boilerplate, redundant TDD cross-reference (simplified to workflow step) |
| **config-verification-patterns** | 5-point config.yaml quick check table, curator verification commands, .env style conventions, Jina MCP quote integrity check, Gemini usage audit, curator hallucination warning | Specific config.yaml grep examples kept but integrated into workflow Track 3 |
