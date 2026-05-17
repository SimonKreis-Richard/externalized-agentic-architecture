# Skills Reference

> *Skills are hub-maintained, cast manually, and installed on demand. No custom skills. No automatic loading. This file is a reference list only — it contains no skill content.*

---

## Install Commands

Run these commands in Hermes to install a skill from the hub. Skills are cast manually when needed — they are never auto-loaded.

```bash
# Web scraping — HTTP fetching, stealth browser, Cloudflare bypass
hermes skills install scrapling

# Document conversion — PDF, Word, Excel, PPT, images, audio → Markdown
hermes skills install markitdown

# Hermes Agent — configure, extend, or contribute to Hermes
hermes skills install hermes-agent

# SVG diagrams — flat, minimal light/dark-aware educational diagrams
hermes skills install concept-diagrams

# GitHub PR workflow — create branches, commit, open PRs, monitor CI
hermes skills install github-pr-workflow

# PowerPoint — create, read, edit .pptx presentations
hermes skills install powerpoint

# Systematic debugging — 4-phase root cause analysis
hermes skills install systematic-debugging
```

---

## Rules

- **Start with zero skills.** Install only what you need, when you need it.
- **Cast manually.** Skills are never auto-loaded. The user invokes them explicitly.
- **No custom skills.** If a behavioral rule is important enough to be permanent, it belongs in `SOUL.md` — not in a skill.
- **Hub-maintained only.** Skills are updated by their authors. Zero maintenance burden for the user.

---

## What Belongs in the SOUL Instead

These behavioral patterns are encoded directly in `SOUL-TEMPLATE.md` and should never be externalized to a skill:

- **Search trinity** (Tavily → Exa → Jina) — search-tool-protocol is obsolete
- **Token discipline** — permanent reflex, not a skill
- **Delegation rules** — permanent reflex, not a skill
- **Fact-checking** — permanent reflex, not a skill
- **Scope & confinement** — permanent guardrail, not a skill
- **Deduplication principles** — permanent reflex, not a skill
