---
name: delegation
description: >-
  Complete agent delegation system: when/how/why to delegate, Kanban orchestration
  and work routing, Kanban worker handoffs and retry patterns, and subagent-driven
  development with two-stage review. Core CEO skill — load every session.
  Déclencheurs FR: « délègue », « subagent », « kanban », « orchestre »,
  « décompose », « parallélise ».
domain: agentic
category: delegation
importance_score: 9
generalization_score: 9
core: false
suggested_loading: auto
author: [USER_NAME] + [AGENT_NAME]
source:
  - skills-curated/custom-core/delegation-methodology/SKILL.md
  - skills-curated/built-in/kanban-orchestrator/SKILL.md
  - skills-curated/built-in/kanban-worker/SKILL.md
  - skills-curated/built-in/subagent-driven-development/SKILL.md
merged_from:
  - delegation-methodology
  - kanban-orchestrator
  - kanban-worker
  - subagent-driven-development
fork_date: 2026-05-27
consumes_official: false
suggested_loading: always
keywords: [delegation, subagent, kanban, orchestration, decomposition, parallel, worker, orchestrator, review, methodology]
triggers: [délègue, subagent, kanban, orchestration, décomposition, parallélise, orchestre]
---

# Delegation — Agent Orchestration Master Skill

> Unifie la méthodologie de délégation, le routage Kanban, les bonnes pratiques worker, et le développement piloté par subagents avec revue systématique.
> Si l'utilisateur dit « délègue », « subagent », « kanban », « orchestre », « décompose » ou « parallélise » — charge cette skill.

---

## When to Load

Load this skill whenever any of these are true:

- The user asks you to **déléguer** (delegate) work to subagents
- You need to **décomposer** (decompose) a large task into parallel workstreams
- You are working with **Kanban** tasks — creating, routing, or completing them
- You need to **orchestrer** (orchestrate) multiple agents with dependencies
- The task requires **paralléliser** (parallelize) independent work
- You are implementing a plan via subagent workers with quality gates
- A worker is stuck, crashed, or needs retry diagnostics
- You are reviewing subagent output — whether spec compliance, code quality, or integration

---

## Overview

This skill merges four complementary sources into a single unified delegation system:

1. **Delegation Methodology** — The CEO funnel: when to delegate, how to decompose, how to write subagent prompts, how to handle failures (3-strike rule), and anti-patterns.
2. **Kanban Orchestrator** — Kanban-based work decomposition: profile discovery, anti-temptation rules, the 5-step decomposition playbook, task graphs with dependencies, parallel fan-out, and stuck worker recovery.
3. **Kanban Worker** — Worker-side behaviour: workspace handling, handoff shapes (summary + metadata), claiming created cards, blocking with context, retry diagnostics, heartbeats, and notification routing.
4. **Subagent-Driven Development** — Per-task implementer + two-stage review (spec compliance first, code quality second), task granularity rules, and integration patterns.

**Core principles:**
- Delegate reasoning-heavy work; do mechanical work yourself
- Default to parallel fan-out; sequential only when there are hard dependencies
- Fresh subagent per task — never reuse context with accumulated state
- Two-stage review every time: spec compliance FIRST, code quality SECOND
- Subagent summaries are self-reports — always verify external side-effects

---

## Workflow

### Phase 1: Delegate or Not? (Decision Tree)

```
Can I do it in ONE tool call?
  ├── YES → Do it yourself. No delegation overhead.
  └── NO → Does it need REASONING?
           ├── NO (mechanical multi-step) → execute_code
           └── YES (reasoning needed) → DELEGATE
```

| Approach | When | Example |
|----------|------|---------|
| **Direct tool call** | 1 call, no logic | `read_file`, `web_search` |
| **execute_code** | 3+ calls, processing loops, filtering | Batch process N files, paginate API |
| **delegate_task** (leaf) | Reasoning-heavy, >2 steps | Debug session, code review, research |
| **delegate_task** (orchestrator) | Multi-level decomposition | Complex plan with sub-workers |
| **kanban_create** | Work that survives crashes, human-in-the-loop, or cross-agent | Multi-specialist pipeline, persistent audit trail |
| **terminal** | System ops, builds, installs, git | `npm install`, `git commit` |

**Anti-pattern:** delegating `read_file` or other single-tool operations. The CEO doesn't hire a consultant to read a file.

### Phase 1.5: Model Routing (Pro vs Flash)

After deciding to delegate, choose the right model for the sub-agent.

**Core principle:** Orchestrator ([AGENT_NAME]/main session) stays on Pro. Execution tasks delegate to Flash.
> **Variables:** `{PRO_MODEL}`, `{FLASH_MODEL}`, `{PROVIDER}` — values in `{VARS_PATH}`.

| Task Type | Model | Why |
|---|---|---|
| Orchestration ([AGENT_NAME]) | `{PRO_MODEL}` | Reasoning + judgment |
| File ops | `{FLASH_MODEL}` | Fast, cheap, tool-calling |
| Web search + extraction | `{FLASH_MODEL}` | Speed matters |
| Terminal commands | `{FLASH_MODEL}` | Mechanical |
| Data processing | `{FLASH_MODEL}` | CPU-bound, not brain-bound |
| Document creation | `{FLASH_MODEL}` | Template work |
| Complex code/debug | `{PRO_MODEL}` | Needs reasoning |
| Research synthesis | `{PRO_MODEL}` | Needs judgment |

**Why this works:** Flash models overthink less, iterate faster, and cost ~85% less on execution tasks. The paradox from benchmarks (Pro models scoring worse on agentic tasks due to overthinking) is resolved by letting Pro only think and Flash only act.

**Pattern:**
```python
# Execution → Flash
delegate_task(
  goal="Read ~/data/sales.csv and calculate monthly totals",
  context="File path: ~/data/sales.csv. Output as JSON.",
  toolsets=["terminal", "file"],
  model={"provider": "{PROVIDER}", "model": "{FLASH_MODEL}"}
)

# Reasoning → Pro
delegate_task(
  goal="Review auth.py for security vulnerabilities",
  context="File: ~/app/auth.py. Focus on injection and auth bypass.",
  toolsets=["terminal", "file"],
  model={"provider": "{PROVIDER}", "model": "{PRO_MODEL}"}
)
```

**Why this works — benchmark evidence:**
- **Overthinking paradox (WolfBench/W&B):** {PRO_MODEL} at max thinking dropped 11.9 points (71.5% → 59.6%). On some tasks, 60 minutes of thinking produced zero actions.
- **PinchBench (May 2026):** Flash models consistently outrank Pro models on agentic tasks. {FLASH_MODEL} > {PRO_MODEL} on agentic benchmarks. {PRO_MODEL} scores drop significantly on agentic tasks.
- **arXiv "The Danger of Overthinking" (Cuadron et al., 2025):** Three failure modes of reasoning models in agentic contexts — analysis paralysis, rogue actions, premature disengagement.
- **Key insight:** *"The value of extended thinking is inversely proportional to the model's baseline agentic capability."* Strong models get WORSE with more reasoning; weak models get BETTER.
- **Resolution:** Separate concerns. Pro thinks (orchestration, planning, synthesis). Flash acts (tool calls, file ops, mechanical work). Both do what they're best at.

**Escalation rule:** If Flash fails 2x on a task, re-delegate to Pro. Don't keep retrying with the wrong model.

---

### Phase 2: Decompose the Task

#### The 3-Question Decomposition

1. **Can this be parallelized?** → Multiple subagents simultaneously (max 3 per batch)
2. **Does this need orchestration?** → One orchestrator spawning workers (max_spawn_depth=3)
3. **What's the minimum viable decomposition?** → Fewest subagents that cover the work

| Pattern | When | Cost |
|---------|------|------|
| **Parallel (tasks=[])** | Independent workstreams | 1 turn, all results together |
| **Sequential** | Output of A feeds B | N turns, builds on results |
| **Orchestrator** | Complex multi-pass analysis | Decomposes, workers execute |

**Rule:** default to parallel. Only sequential if there's a hard dependency chain.

#### Task Granularity

Each task = 2-5 minutes of focused work.

- **Too big:** "Implement user authentication system"
- **Right size:** "Create User model with email + password fields", "Add password hashing function", "Create login endpoint"

#### Kanban-Specific Decomposition

If using Kanban (work should survive crashes, needs human-in-the-loop, or multi-profile routing), follow the 5-step playbook:

**Step 0 — Discover available profiles.** Run `hermes profile list` or ask the user what profiles exist. Never invent profile names — the dispatcher silently drops unknown assignees.

**Step 1 — Understand the goal.** Ask clarifying questions if ambiguous. Cheaper to ask than to spawn the wrong fleet.

**Step 2 — Sketch the task graph.** Before creating anything:
1. Extract lanes from the request
2. Map each lane to a discovered profile
3. Decide independence vs. dependency
4. Independent lanes = parallel cards with no parent links
5. Dependent lanes = synthesis/review/integration cards with `parents=[...]`

Words like "also", "finally", "and" do NOT automatically imply a dependency. Only link tasks when one card cannot start without the other's output.

**Step 3 — Create tasks and link.** Create parent cards first, capture their IDs, pass them in `parents=` for child cards. Use tenant namespacing: `tenant=os.environ.get("HERMES_TENANT")`.

**Step 4 — Complete your own task.** If you were spawned as a planner/orchestrator, mark your task done with a summary and task graph metadata.

**Step 5 — Report.** Tell the user what you created, naming actual profiles.

### Phase 3: Write the Subagent Prompt

The subagent is a **coquille vide** — no SOUL, no memory, no conversation history. Everything it needs MUST be in the prompt.

| Field | Content | Length |
|-------|---------|--------|
| **goal** | WHAT to accomplish. 1-2 sentences. Action-oriented. | Short |
| **context** | Everything it needs: file paths, error messages, project structure, constraints, language instruction | As long as needed |

**Critical Context Checklist:**
- [ ] Absolute file paths (subagent has different cwd)
- [ ] Relevant error messages / stack traces
- [ ] Project conventions and constraints
- [ ] **Language instruction** — if user writes in FR, tell subagent "respond in French"
- [ ] Expected output format
- [ ] What NOT to do
- [ ] Explicit "must-have" truths the subagent must confirm (not just "did a file appear?")

**Language Propagation Rule:** Si l'utilisateur parle français → `context: "Réponds en français. [reste du contexte]"`. Sans cette instruction, les subagents répondent en anglais.

**Never inline large files into subagent prompts.** Tell the subagent to `read_file` from disk instead.

### Phase 4: Select Role and Toolsets

| Role | Capabilities | When to use |
|------|-------------|-------------|
| **leaf** (default) | Tools, no delegate_task | Simple to moderate tasks. ~95% of cases. |
| **orchestrator** | Can spawn own workers (delegate_task) | Multi-pass analysis, nested research |

**Constraints:** `max_spawn_depth=3` → CEO → orchestrator → workers (2 levels deep max). Depth 2 suffit 95% du temps.

**Toolsets — give only what's needed:**

| Task type | Minimal toolsets |
|-----------|-----------------|
| Code work | `["terminal", "file"]` |
| Research | `["web"]` or `["search"]` |
| Full-stack | `["terminal", "file", "web"]` |
| System ops | `["terminal", "file"]` |

**Anti-pattern:** giving a code-review subagent `["web"]` — it doesn't need web access and the tool descriptions waste tokens.

### Phase 5: Execute with Two-Stage Review (Subagent-Driven Development)

For EACH task in a plan:

#### Step 1: Dispatch Implementer Subagent

```python
delegate_task(
    goal="Implement Task 1: Create User model with email and password_hash fields",
    context="""
    TASK FROM PLAN:
    - Create: src/models/user.py
    - Add User class with email (str) and password_hash (str) fields
    - Use bcrypt for password hashing
    - Include __repr__ for debugging

    FOLLOW TDD:
    1. Write failing test in tests/models/test_user.py
    2. Run: pytest tests/models/test_user.py -v (verify FAIL)
    3. Write minimal implementation
    4. Run: pytest tests/models/test_user.py -v (verify PASS)
    5. Run: pytest tests/ -q (verify no regressions)
    6. Commit: git add -A && git commit -m "feat: add User model"

    PROJECT CONTEXT:
    - Python 3.11, Flask app in src/app.py
    - Tests use pytest, run from project root
    - bcrypt already in requirements.txt
    """,
    toolsets=['terminal', 'file']
)
```

#### Step 2: Dispatch Spec Compliance Reviewer

After implementer completes, verify against the original spec:

```python
delegate_task(
    goal="Review if implementation matches the spec from the plan",
    context="""
    ORIGINAL TASK SPEC:
    - Create src/models/user.py with User class
    - Fields: email (str), password_hash (str)
    - Use bcrypt for password hashing
    - Include __repr__

    CHECK:
    - [ ] All requirements from spec implemented?
    - [ ] File paths match spec?
    - [ ] Function signatures match spec?
    - [ ] Behavior matches expected?
    - [ ] Nothing extra added (no scope creep)?

    OUTPUT: PASS or list of specific spec gaps to fix.
    """,
    toolsets=['file']
)
```

**If spec issues found:** Fix gaps, re-run spec review. Continue only when spec-compliant. Do NOT start quality review before spec compliance is PASS.

#### Step 3: Dispatch Code Quality Reviewer

```python
delegate_task(
    goal="Review code quality for Task 1 implementation",
    context="""
    FILES TO REVIEW:
    - src/models/user.py
    - tests/models/test_user.py

    CHECK:
    - [ ] Follows project conventions and style?
    - [ ] Proper error handling?
    - [ ] Clear variable/function names?
    - [ ] Adequate test coverage?
    - [ ] No obvious bugs or missed edge cases?
    - [ ] No security issues?

    OUTPUT FORMAT:
    - Critical Issues: [must fix before proceeding]
    - Important Issues: [should fix]
    - Minor Issues: [optional]
    - Verdict: APPROVED or REQUEST_CHANGES
    """,
    toolsets=['file']
)
```

**If quality issues found:** Fix issues, re-review. Continue only when approved.

#### Step 4: Final Integration Review

After ALL tasks are complete, dispatch a final integration reviewer:

```python
delegate_task(
    goal="Review the entire implementation for consistency and integration issues",
    context="""
    All tasks from the plan are complete. Review the full implementation:
    - Do all components work together?
    - Any inconsistencies between tasks?
    - All tests passing?
    - Ready for merge?
    """,
    toolsets=['terminal', 'file']
)
```

### Phase 6: Verify Subagent Output

Subagent summaries are **SELF-REPORTS, not verified facts.** A subagent claiming success may be wrong.

**Verification rules:**
- Require verifiable handles from subagents: absolute paths, URLs, task IDs
- For side-effect operations (file writes, HTTP calls): read back the file or fetch the URL yourself
- For Kanban tasks: verify `kanban_create` return values — never invent task IDs
- Do not trust "uploaded successfully" without checking

### Phase 7: Handle Failures (3-Strike Rule)

```
Échec 1 → Retry with adjusted context
Échec 2 → Retry with different approach
Échec 3 → STOP → Root cause analysis. DO NOT retry.
```

**After 3 failures:**
1. Read subagent summaries — what went wrong?
2. Identify root cause (wrong toolsets? insufficient context? task too complex?)
3. Restructure the task (split differently, more context, different role)
4. If systemic → escalate to user

**Never:** retry the same prompt with the same parameters.

---

## Kanban Worker Handoff Patterns

### Good Summary + Metadata Shapes

**Coding task:**
```python
kanban_complete(
    summary="shipped rate limiter — token bucket, keys on user_id with IP fallback, 14 tests pass",
    metadata={
        "changed_files": ["rate_limiter.py", "tests/test_rate_limiter.py"],
        "tests_run": 14,
        "tests_passed": 14,
        "decisions": ["user_id primary, IP fallback for unauthenticated requests"],
    },
)
```

**Coding task needing human review (review-required):**
```python
kanban_comment(
    body="review-required handoff:\n" + json.dumps({
        "changed_files": ["rate_limiter.py", "tests/test_rate_limiter.py"],
        "tests_run": 14,
        "tests_passed": 14,
        "diff_path": "/path/to/worktree",
        "decisions": ["user_id primary, IP fallback for unauthenticated requests"],
    }, indent=2),
)
kanban_block(
    reason="review-required: rate limiter shipped, 14/14 tests pass — needs eyes on the user_id/IP fallback choice before merging",
)
```

**Research task:**
```python
kanban_complete(
    summary="3 competing libraries reviewed; vLLM wins on throughput, SGLang on latency",
    metadata={
        "sources_read": 12,
        "recommendation": "vLLM",
        "benchmarks": {"vllm": 1.0, "sglang": 0.87, "trtllm": 0.72},
    },
)
```

**Review task:**
```python
kanban_complete(
    summary="reviewed PR #123; 2 blocking issues found (SQL injection in /search, missing CSRF)",
    metadata={
        "pr_number": 123,
        "findings": [
            {"severity": "critical", "file": "api/search.py", "line": 42, "issue": "raw SQL concat"},
        ],
        "approved": False,
    },
)
```

Shape `metadata` so downstream parsers can use it without re-reading prose.

### Workspace Handling

| Kind | What it is | How to work |
|------|-----------|-------------|
| `scratch` | Fresh tmp dir, yours alone | Read/write freely; GC'd on archive |
| `dir:<path>` | Shared persistent directory | Treat like long-lived state |
| `worktree` | Git worktree at resolved path | Run `git worktree add` if `.git` missing; commit work here |

### Claiming Created Cards

Only list IDs you captured from successful `kanban_create` return values:
- **GOOD:** `created_cards=[c1["task_id"], c2["task_id"]]`
- **BAD:** inventing IDs from prose — the gate rejects them

### Block Reasons That Get Answered Fast

Bad: `"stuck"` — human has no context.
Good: `"Rate limit key choice: IP (simple, NAT-unsafe) or user_id (requires auth, skips anonymous endpoints)?"`

Leave longer context as a `kanban_comment`. The block reason is what appears in the dashboard.

### Retry Scenarios

Check `kanban_show` → `runs: [...]` for prior outcomes:

| Outcome | Meaning | Action |
|---------|---------|--------|
| `timed_out` | Hit max runtime | Chunk work or shorten it |
| `crashed` | OOM or segfault | Reduce memory footprint |
| `spawn_failed` | Config issue (credential, PATH) | Block and ask human |
| `reclaimed` | Archived out from under previous run | Check status carefully |
| `blocked` | Previous attempt blocked | Check thread for unblock comment |

---

## Tools & Commands

### delegate_task (subagent spawning)

```python
# Single task
delegate_task(goal="...", context="...", toolsets=['terminal', 'file'])

# Parallel batch (up to 3)
delegate_task(tasks=[
    {"goal": "Research A", "context": "...", "toolsets": ["web"]},
    {"goal": "Research B", "context": "...", "toolsets": ["web"]},
])
```

### Kanban Tools (from within Kanban workers)

| Tool | Purpose | CLI Equivalent |
|------|---------|----------------|
| `kanban_show` | Read task details + run history | `hermes kanban show <id> --json` |
| `kanban_complete` | Mark task done with summary + metadata | `hermes kanban complete <id> --summary "..." --metadata '{}'` |
| `kanban_block` | Block task, wait for human | `hermes kanban block <id> "reason"` |
| `kanban_create` | Create child task, assign to profile | `hermes kanban create "title" --assignee <profile>` |
| `kanban_comment` | Add comment to task thread | `hermes kanban comment <id> "message"` |
| `kanban_link` | Link parent → child dependency | `hermes kanban link <parent_id> <child_id>` |

### Recovering Stuck Workers

| Action | CLI | What it does |
|--------|-----|--------------|
| Reclaim | `hermes kanban reclaim <task_id>` | Abort running worker, reset to `ready` |
| Reassign | `hermes kanban reassign <task_id> <profile> --reclaim` | Switch to different profile |
| Change model | `hermes -p <profile> model <new-model>` | Edit profile config, then Reclaim |

---

## Pitfalls

| Pitfall | Why it fails | Instead |
|---------|-------------|---------|
| Delegating a single tool call | Overhead > value | Just do it yourself |
| Missing language context | Subagent replies in English, contaminates FR conversation | Always add language instruction to context |
| Over-delegating (5+ subagents for simple task) | Coordination overhead > execution time | Use max 3, or use execute_code |
| Delegating user interaction | Subagents cannot use clarify | Handle questions yourself, then delegate |
| Trusting subagent summaries blindly | They can hallucinate success | Verify side-effect operations |
| Same prompt after failure | Definition of insanity | Change something before retrying |
| Giving all toolsets to everyone | Wasted context tokens | Minimal toolsets: only what's needed |
| Inventing profile names that don't exist | Dispatcher silently drops unknown assignees | Discover profiles first via `hermes profile list` |
| Bundling independent lanes into one Kanban card | Creates single points of failure | One card per independent workstream |
| Over-linking because of wording | "Finally check X" may still be parallel | Link only when output is needed as input |
| Creating dependent work as independent ready cards | Race conditions | Use `parents=[...]` to gate promotion |
| Forgetting dependency links | Implement/review run before inputs exist | Always link at creation time |
| Reassign vs. new task on review failure | Re-running same task doesn't fix root cause | Create NEW task linked from reviewer |
| Skipping spec compliance review | Missing requirements compound | Spec review FIRST, quality review SECOND |
| Starting quality review before spec passes | Wasted review cycles | Always spec review first |
| Letting subagent self-review | Blind spots | Both spec AND quality reviewer needed |
| Proceeding with unfixed critical issues | Bug cascade | Block until all issues resolved |
| Claiming cards by invented IDs | Gate rejects completion | Only claim IDs captured from `kanban_create` |
| Calling `delegate_task` as substitute for `kanban_create` | Cross-agent handoffs need persistence | `kanban_create` for tasks that outlive one API loop |
| Relying on CLI in containerized backends | CLI not installed | Use the `kanban_*` tools instead |
| Skipping final integration review | Component mismatch | Always dispatch final review after all tasks |
| Assuming `max_iterations` is the bottleneck | Workers often hit `child_timeout_seconds` first, not iteration limit. A 600s timeout with 5-10s per iteration = only 60-120 effective iterations | When a worker runs out of steam, check elapsed time vs timeout before assuming iterations were insufficient. Increase `child_timeout_seconds` first |

---

## Verification Checklist

- [ ] Did you use the decision tree before delegating? (Single tool call first?)
- [ ] Did you provide absolute file paths in context?
- [ ] Did you include language instruction (FR → "respond in French")?
- [ ] Did you give minimal toolsets (no unused tools)?
- [ ] Did you use parallel `tasks=[]` for independent work?
- [ ] Did you verify subagent side-effect operations (read back files, fetch URLs)?
- [ ] Did you apply the 3-strike rule (change approach after 2 failures)?
- [ ] If Kanban: did you discover profiles before assigning?
- [ ] If Kanban: did you use `parents=[...]` for dependent tasks?
- [ ] If Kanban: did you pass `tenant=os.environ.get("HERMES_TENANT")`?
- [ ] If building from a plan: did you run spec compliance review FIRST?
- [ ] If building from a plan: did you run code quality review SECOND?
- [ ] If building from a plan: did you run final integration review?
- [ ] Are all subagent-claimed task IDs verified?
- [ ] Are all issues resolved before marking complete?

---

## References

- **`references/hermes-pitfalls.md`** — Hermes-specific pitfalls when creating skills (from delegation-methodology)
- **`references/context-budget-discipline.md`** — Four-tier context degradation model, read-depth rules, early warning signs (from subagent-driven-development)
- **`references/gates-taxonomy.md`** — Four canonical gate types: Pre-flight, Revision, Escalation, Abort (from subagent-driven-development)

---

## Merged Sources

This skill merges the following four skills, preserving unique insights and avoiding duplication:

1. **delegation-methodology** — CEO funnel decision tree, goal/context split, leaf vs orchestrator, 3-strike failure handling, anti-patterns → kept in full as foundational sections
2. **kanban-orchestrator** — Profile discovery, board vs delegate_task decision, 5-step decomposition playbook, task graph patterns, stuck worker recovery → kept as Kanban-specific sections
3. **kanban-worker** — Workspace handling, handoff shapes, claiming cards, block reasons, retry diagnostics → kept as worker-focused reference sections
4. **subagent-driven-development** — Per-task implementer + 2-stage review, task granularity, red flags → kept as the implementation execution methodology

**Discarded/consolidated:**
- Duplicate anti-patterns (e.g., "don't delegate single tool calls" appeared in multiple sources → single entry)
- Duplicate verification rules merged into one Phase 6
- Platform-specific notes consolidated
- Redundant "related skills" cross-references removed (they live in the broader skill ecosystem, not here)
- KANBAN_GUIDANCE references (auto-injected by system prompt, not a skill concern)
