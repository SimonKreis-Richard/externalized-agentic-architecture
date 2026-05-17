# Information Flow: Active Externalization, Data Pipelines & the Local-First Paradigm

## The Problem: Digital Entropy at Scale

Modern knowledge work generates noise faster than any organizational system can filter it. GTD and PARA worked before the agentic era, but AI agents — Claude Code, OpenClaw, Hermes — are *exponential file generators*. They produce output continuously, rarely clean up, and compound digital chaos. The result is a new class of problem: information entropy accelerated by the very tools meant to reduce it.

For OCD-prone minds, this is paralyzing. For the overwhelmed, it's buried under the rug. For both, the solution is the same principle: **fight fire with fire** — impose discipline through configuration, within the agentic paradigm itself.

## Active Externalization

The core architectural insight: **skills, memory, personality, and context must live outside the scaffolding infrastructure.** Identity and objectives are externalized to portable files (JSON, Markdown) in a flat, centralized directory — not embedded in any single agent's config folder. This means:

- Agent identity is a file (`AGENTS.md`, `SOUL.md`), not a database entry.
- User knowledge domains are factual JSON databases — queryable by AI, deduplicated by design.
- Configuration is a template, reproducible across machines, migratable with `git push`.

Externalization buys *portability*, *reproducibility*, and *parallel agent support* (Hermes + Claude Code on the same data). It also means the agent's *state* (conversations, checkpoints) is separated from its *knowledge* (facts, identity, skills).

## The Data Pipeline: Chaotic Capture → Distillation → Categorization → Routing

Information enters messy and leaves structured. The pipeline has four stages:

1. **Chaotic Capture** — Raw notes, bookmarks, scraps, voice memos, screenshots. Everything lands in the **Inbox** zone. No structure required.
2. **Distillation** — AI agents (Hermes, Claude Code) process raw captures → extract facts, summaries, links, decisions. Deduplication against existing knowledge bases (JSON stores) happens here.
3. **Categorization** — Distilled facts are tagged, typed, and slotted into knowledge domains. Tags become routing keys.
4. **Routing** — Structured output lands in the **Playground** zone (workspace for agent action) or, after human review, is promoted to the **Vault** (permanent reference).

This is a *funnel*, not a bucket. Each stage reduces volume and increases signal.

## The 3-Zone Model: Inbox / Playground / Vault

| Zone | Purpose | Agent Write Access | Human Review |
|------|---------|--------------------|--------------|
| **Inbox** | Raw capture, unfiltered | Read + write | None (yet) |
| **Playground** | Agent workspace, active projects | Full read/write | Periodic |
| **Vault** | Permanent knowledge, facts, archives | Read-only | Required before promotion |

**Critical rule:** Agents write *only* to Playground. The Vault is human-stewarded. This prevents the agent from polluting durable knowledge with transient output, while giving it full freedom to work. The Playground is where agents build, draft, and iterate; the Vault is where curated knowledge lives.

## The Local-First Paradigm Shift

The cloud is transforming from *data custodian* to *inference provider*. For a decade, the rule was "move data to the cloud for integrated experience." The new pattern inverts this: **AI is an API reaching into local data.**

Consequences:

- Personal data stays in plain text on the local machine (or self-hosted). No more vendor lock-in for storage.
- AI agents interact with local file systems directly — no Docker, no SSH gymnastics, no cloud middleware.
- Decentralized AI access: multiple agents (Hermes, Claude Code, future agents) all read the same local data, configured differently.
- Sync happens via portable tools (Syncthing, Git) — *data* syncs, *agent state* does not.

This is paradoxical and powerful: AI is driving *data decentralization*, not centralization. The cloud becomes a commodity inference layer; value accrues to the local data architecture.

## Plain Text, Portable Data

Why Markdown + JSON, and nothing else?

- **LLM-native**: Agents read and write text trivially. No parsing hell, no binary blobs, no proprietary formats.
- **Queryable**: JSON knowledge bases are directly filterable by agents. Fact retrieval is a `jq` or code call away.
- **Versionable**: Git works natively. Diffs are meaningful. History is recoverable.
- **Portable**: A single flat mega-directory with subfolders. Move it to any OS, any machine, any agent — it just works.
- **Externalizable**: Personality (`SOUL.md`), skills (`skills/`), memories (`memories/`), config (`config.yaml` + `.env`) — all files, all portable, all syncable without agent lock-in.

The rule: **everything that can be text, is text.** The only exceptions are media files referenced by path. This guarantees that the data layer outlives any single agent or scaffolding tool.

## File Layout (Conceptual)

```
~/hermes/
├── AGENTS.md               # Core identity / system prompt
├── SOUL.md                 # Personality, goals, constraints
├── config.yaml             # Agent configuration
├── .env                    # API keys (excluded from Git)
├── Inbox/                  # Raw captures
├── Playground/             # Agent working directory
│   ├── projects/
│   └── drafts/
├── Vault/                  # Curated knowledge
│   ├── knowledge/          # JSON domain databases
│   ├── references/         # Important documents
│   └── archive/
├── skills/                 # MCP skill definitions (YAML)
├── memories/               # Agent memories (text)
└── auth.json               # Credential manifests
```

## Summary

Active externalization + the Inbox/Playground/Vault funnel + local-first plain text = a data architecture designed for the agentic era. It imposes discipline where AI creates chaos, preserves portability where the industry pushes lock-in, and inverts the cloud relationship from custodian to mere inference pipe. The result is a system where information flows naturally from chaos to structure, where agents amplify rather than overwhelm, and where the user remains the steward of their own knowledge.
