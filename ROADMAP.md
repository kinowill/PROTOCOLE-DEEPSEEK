# Roadmap — PROTOCOLE DEEPSEEK

> Backlog de référence du projet.
> Mis à jour à chaque chantier terminé ou réorienté.

Dernière mise à jour : 2026-04-24

---

## Contexte

Le repo porte la version DeepSeek/opencode du protocole, son guide d'intégration
et l'outil `show_context.ps1` (adapté pour fenêtre 1M). Le cap initial est de
publier une v1.0 alignée avec les protocoles Claude (v1.2) et Codex (v1.3).

## Hypothèses retenues

- `README.md` sert de document maître opératif du repo.
- `PROTOCOLE.md` est la source canonique à injecter dans `~/.claude/CLAUDE.md`.
- `show_context.ps1` lit les `.jsonl` opencode (même emplacement que Claude Code).
- DeepSeek V4 Pro = 1M tokens de contexte (source : api-docs.deepseek.com).
- `rtk` et `gh` sont des outils optionnels de workflow.

## État courant

- 2026-04-24 : adaptation approfondie avec les spécificités DeepSeek (thinking
  mode, context caching disque, tool calling parallèle, rate limiting) et
  opencode (AGENTS.md, agents Build/Plan/General/Explore, skills, LSP,
  commandes slash, agent de compaction).

## Priorités hautes

Format : `[ ]` à faire, `[~]` partiellement fait, `[x]` fait.

### [x] HP1 — Publier la v1.0 du protocole DeepSeek

**Objectif** :
- Créer un protocole canonique pour DeepSeek V4 Pro via opencode, aligné
  sur les versions Claude et Codex existantes.

**Actions** :
- Rédiger `PROTOCOLE.md` avec les 14 sections canoniques adaptées.
- Créer `integrations/opencode.md` pour l'injection via `CLAUDE.md`.
- Adapter `show_context.ps1` pour la fenêtre 1M.
- Créer les templates standard (maître, roadmap, validation).

**Livrables** :
- Dossier complet : `PROTOCOLE.md`, `README.md`, `ROADMAP.md`, `CHANGELOG.md`,
  `integrations/opencode.md`, `show_context.ps1`, `templates/`, `.gitignore`,
  `LICENSE`.

**Critère de fin** :
- Un utilisateur peut installer opencode + protocole + `show_context.ps1` et
  voir le protocole actif dès la première session.

## Priorités moyennes

### [ ] MP1 — Tester l'installation sur un projet vierge

**Objectif** :
- Valider que le protocole est chargé correctement dans une session opencode
  neuve, sur un projet sans document maître.

**Actions** :
- Créer un projet test sans `DOCUMENT_MAITRE.md`.
- Lancer opencode avec le protocole dans `~/.claude/CLAUDE.md`.
- Vérifier que le protocole de début de session se déclenche.

**Livrables** :
- Résultat du test consigné dans le journal de validation ou la roadmap.

**Critère de fin** :
- opencode propose de créer le maître avant d'agir sur le chantier demandé.

### [ ] MP2 — Portage du monitoring hors Windows

**Objectif** :
- Étendre `show_context.ps1` ou fournir un équivalent pour macOS / Linux.

**Actions** :
- Étudier l'emplacement des `.jsonl` opencode sur les autres OS.
- Porter le script en bash ou Python.

**Livrables** :
- Script ou doc d'alternative cross-platform.

**Critère de fin** :
- Un utilisateur non-Windows peut contrôler le contexte sans bricolage manuel.

## Priorités basses

### [ ] BP1 — Ajouter un exemple de projet sensible

**Objectif** :
- Montrer comment documenter auth, données utilisateurs ou prod sensible
  dans un `CLAUDE.md` projet.

**Actions** :
- Rédiger un exemple bref mais réaliste.

**Livrables** :
- Exemple intégré à `integrations/opencode.md` ou à un template dédié.

**Critère de fin** :
- Le template montre clairement comment cadrer une zone sensible.

## Ordre recommandé d'exécution

1. Publier la v1.0 (fait).
2. Tester l'installation sur un projet vierge.
3. Porter le monitoring hors Windows.
4. Ajouter un exemple de projet sensible.

## Definition of done pour le prochain cap

Le projet franchit un cap solide quand :
- l'installation globale, projet et hybride donne les mêmes règles ;
- le monitoring de contexte fonctionne sous Windows et est documenté ;
- les outils optionnels (`rtk`, `gh`) sont cadrés sans devenir des dépendances cachées.

## Prochaine action recommandée

Tester l'installation dans un repo vierge avec `~/.claude/CLAUDE.md` contenant
le protocole complet, puis consigner le résultat dans un journal de validation.
