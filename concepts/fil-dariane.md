# Le Fil d'Ariane — Traçabilité Cognitive

> *« Donne-moi un fil, et je retrouverai mon chemin dans n'importe quel labyrinthe. »*
>
> Ariane donne un fil à Thésée. Le labyrinthe est la complexité cognitive. Le fil est chaque externalisation qui laisse une trace traçable.

---

## 1. La Métaphore

Dans le mythe grec, Thésée entre dans le labyrinthe du Minotaure. Ariane lui remet un fil qu'il déroule à mesure qu'il avance. Quand il a tué la bête, il remonte le fil jusqu'à la sortie.

Dans notre architecture, **le labyrinthe est la complexité cognitive** : une chaîne de décisions, de délégations, de recherches, de synthèses, d'exécutions — chaque étape est un couloir qui pourrait être une impasse. **Le fil est la trace** : chaque externalisation laisse un jeton durable (commit, fichier, entrée mémoire) que le système peut suivre en sens inverse pour retrouver l'origine de n'importe quelle décision.

Sans fil, Thésée meurt dans le labyrinthe. Sans traçabilité, un agent cognitif devient une boîte noire.

---

## 2. Implémentation Concrète

Le fil n'est pas une métaphore abstraite — il est tissé par des mécanismes précis, visibles et opérationnels :

| Mécanisme | Ce qu'il produit | Comment il tisse le fil |
|---|---|---|
| **Git commits** | `git log --oneline` | Chaque commit documente *pourquoi* un changement a eu lieu, avec référence à la session |
| **Champ `priority:`** | Métadonnée dans les skills/entrées | Indique l'importance relative d'une trace dans le labyrinthe |
| **`MEMORY.md`** | Entrées datées et typées | Une trace stable qui traverse les sessions — la mémoire durable du fil |
| **`agents/YYYY-MM-DD_desc.ext`** | Fichier horodaté par session | Chaque sortie de session est un nœud sur le fil |
| **`session_search()`** | Index de toutes les sessions passées | Permet de remonter instantanément n'importe quel segment du fil |
| **Logs cron** | Horodatage des audits automatiques | Le fil se tisse même en l'absence d'utilisateur — autonomie tracée |
| **`priority:` + tags** | Métadonnées de classification | Organise les brins du fil par importance et par domaine |

Chacun de ces mécanismes produit un **jeton durable** — un point d'ancrage sur le fil que l'on peut retrouver, lire, et suivre.

---

## 3. Taxonomie des Traces

Toute action cognitive produit une trace. La question n'est pas *si* elle est tracée, mais *comment* y remonter.

| Acte | Trace produite | Comment remonter |
|---|---|---|
| Création d'un skill | Fichier dans `skills/` + Git commit | `git log --follow skills/nom-du-skill.md` |
| Décision de design | Entrée dans `MEMORY.md` | `grep -r "décision" MEMORY.md` + date |
| Session de recherche | `agents/2026-05-19_recherche-X.md` | `session_search("recherche-X")` |
| Audit automatique | Ligne dans log cron | `crontab -l` + grep de la date |
| Modification de projet | Git commit avec message | `git show <hash>` |
| Délégation à sous-agent | Fichier de résultat horodaté | `ls agents/2026-05-19_*` |
| Capture d'apprentissage | Entrée dans skill ou mémoire | `priority:` filter + `grep` |
| Pattern détecté (3e usage) | Skillify → skill réutilisable | `git log --all --grep="skillify"` |

**Principe** : si un acte ne produit pas de trace remontable, il n'existe pas pour le système futur.

---

## 4. Pourquoi C'est Critique — Confiance et Auditabilité

### Confiance

Un système dont on ne peut pas tracer les décisions n'inspire pas confiance. Le fil d'Ariane rend le système **transparent** :

- L'utilisateur peut examiner *pourquoi* une décision a été prise en remontant les traces
- Chaque externalisation est vérifiable : le commit montre l'état avant/après
- Les décisions automatiques (cron, audits) laissent des traces lisibles

### Auditabilité

Le fil permet de répondre à des questions critiques :

- « Quand cette règle a-t-elle été introduite ? » → `git log -S "règle"` + session
- « Qui (quel agent) a pris cette décision ? » → commit author + fichier session
- « Quelles alternatives ont été envisagées ? » → `session_search()` sur la période
- « Ce pattern a-t-il déjà été testé ? » → `grep -r` dans les skills + mémoire
- « Pourquoi ce skill a-t-il été modifié la dernière fois ? » → `git show <hash>`

Sans fil, ces questions restent sans réponse. Le système est une boîte noire.

---

## 5. Relation avec les Quatre Piliers

### Externalisation → Créer le fil

Chaque acte d'externalisation (écrire un fichier, commiter, créer un skill) **dépose un nouveau brin** sur le fil. Plus on externalise, plus le fil est dense, plus la traçabilité est fine. L'externalisation *est* l'acte de tisser.

### Autonomie → Le fil se tisse seul

Le système ne dépend pas de l'utilisateur pour tisser le fil. Les audits cron, les skillifications automatiques, les captures de session — chaque mécanisme autonome **ajoute un nœud** au fil. L'autonomie signifie que le fil continue de s'allonger même quand personne ne regarde.

### Funnel → Le fil se condense

À chaque étape du funnel (chaos → distillation → catégorisation → connaissance structurée), le volume d'information diminue mais la **densité de sens** augmente. Le fil suit la même transformation : une trace brute (log) devient une trace structurée (entrée mémoire) devient une trace canonique (skill). Le fil se condense sans se casser.

### Recyclage → Le fil ne casse pas

Le recyclage empêche la **rupture du fil**. Un insight capturé en session devient une entrée mémoire ; une procédure utilisée trois fois devient un skill ; un skill obsolète est consolidé plutôt que supprimé. Le fil ne meurt pas — il se transforme. Chaque recyclage est une continuation, pas une rupture.

---

## 6. Le Fil comme Propriété Émergente

Le fil d'Ariane n'est pas une fonctionnalité qu'on ajoute. C'est une **propriété émergente** d'un système qui externalise radicalement, agit avec autonomie, condense par funnel, et recycle sans casser.

Quand ces quatre piliers fonctionnent ensemble, la traçabilité apparaît *par construction*. Le fil est déjà là — il suffit de savoir le suivre.

---

> *« Le système n'est pas une boîte noire. C'est un labyrinthe dont chaque couloir est un commit, chaque sortie est un fichier, et le fil qui relie tout est l'historique des externalisations. »*
>
> — MANIFESTO.md, v5.0.0

---

**Version** : 1.0.0
**Date** : 2026-05-19
**Aligné avec** : MANIFESTO.md v5.0.0
**Concepts liés** : [information-flow.md](information-flow.md), [MANIFESTO.md](../MANIFESTO.md)
