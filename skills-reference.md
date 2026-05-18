# Skills Reference

> *Skills are externalized procedural memory. They are hub-maintained or custom-built, installed via CLI, and cast manually. No automatic loading. No skill bloat.*

---

## Philosophy

Skills are the agent's **externalized procedural memory**. They live outside the context window and are loaded only when needed. This keeps the SOUL lean (~800 tokens) and the context window free for actual reasoning.

**The 3-uses rule**: a procedure must be executed 3 times before it qualifies for skillification. Premature skill creation is context debt.

**The recycling rule**: when skills overlap, they are consolidated by `skill-recycler`. No duplication. No drift.

---

## Custom Skills (Sam's Own)

These live in `0-custom-skills/` and are Sam's externalized cognitive patterns:

| Skill | Purpose |
|---|---|
| `system-architecture` | Manifeste d'architecture — the SSOT for the entire system design |
| `system-autonomy` | Orchestrateur central d'autonomie — cron jobs, triggers, thresholds |
| `distillation-cycle` | Simon's cognitive signature — DÉMÊLER→DISTILLER→DÉCIDER |
| `skill-design` | Comment bien concevoir un skill — quand créer, quand patcher |
| `skill-recycler` | Détection de duplication et consolidation de skills |
| `soul-audit` | Audit périodique du SOUL contre le paradigme de design |
| `soul-management` | Méthodologie complète de design du SOUL |
| `learnings-capture` | Capture des apprentissages durables après sessions significatives |
| `hub-scout` | Recherche proactive de nouveaux skills sur le hub |
| `hermes-system-maintenance` | Maintenance unifiée du système Hermes (audit + nettoyage) |
| `search-tool-protocol` | Protocole de sélection d'outil de recherche |
| `web-research` | Patterns de recherche web efficace |
| `byterover-memory` | Utilisation avancée de ByteRover comme mémoire persistante |
| `prism` | Analyse unifiée avec 5 sous-modes |
| `metacognition` | Auto-questionnement structuré |
| `data-distillation` | Distillation radicale style Apple |
| `markitdown` | Conversion de documents vers Markdown |
| `corporate-communication-review` | Révision de communications professionnelles |
| `supplement-stack-analysis` | Analyse de stacks de suppléments/nootropiques |
| `financial-data-pipeline` | Pipeline de données financières |
| `pci-methodology` | Permanent Capital Index — sélection two-gate |
| `oracle-ai-agent-config-pattern` | Pattern de configuration pour Oracle HCM |
| `hermes-billing-cv` | Génération de CV professionnel bilingue |

---

## Third-Party Skills (Hub-Installed)

Install via `hermes skills install --yes <skill-identifier>`. These are externalized and community-maintained — zero maintenance burden.

### Méta-Système
```bash
hermes skills install --yes skill-creator              # Création/modification de skills avec evals (Anthropic)
hermes skills install --yes anthropics/skills/skills/brainstorming  # Brainstorming structuré (Anthropic)
```

### Productivité & Design
```bash
hermes skills install --yes anthropics/skills/skills/canvas-design   # Design UI/landing pages HTML
hermes skills install --yes https://github.com/kepano/defuddle       # Nettoyage web token-efficient
```

### Connaissance (nécessite GITHUB_TOKEN dans .env)
```bash
hermes skills install --yes anthropics/knowledge-skills/skills/data-context-engineering/SKILL
hermes skills install --yes anthropics/knowledge-skills/skills/data-visualization/SKILL
hermes skills install --yes anthropics/knowledge-skills/skills/explore-data/SKILL
hermes skills install --yes anthropics/knowledge-skills/skills/documentation/SKILL
```

### Bundled Skills (fournis avec Hermes, restaurés par `hermes update`)
Ces skills sont automatiquement disponibles. Pas besoin d'installation. Liste complète dans le [recovery manifest](hermes-skills-recovery-manifest.md).

---

## What Does NOT Belong in a Skill

These patterns are **permanent reflexes** encoded in the SOUL — never externalized to a skill:

- **Search priority** (Tavily → Exa → Jina)
- **Token discipline**
- **Delegation rules**
- **Fact-checking**
- **Scope & confinement**
- **Externalization triggers** (propose learnings-capture, brv_curate decisions)

If a behavioral rule is important enough to be permanent, it belongs in `SOUL.md` — not in a skill.

---

**Version**: 4.0.0  
**Last updated**: 2026-05-17
