# Roadmap — PROTOCOLE DEEPSEEK

> Backlog de référence du projet.
> Mis à jour à chaque chantier terminé ou réorienté.

Dernière mise à jour : 2026-09-13

---

## Contexte

Le repo porte la version DeepSeek/opencode du protocole, son guide d'intégration
et l'outil `show_context.ps1` (fenêtre 1M). Le cap actuel est de rester aligné
sur PROTOCOLE-CODEX (v1.4, révision `a24f2443f64b09932a27db7343a87f6b18b86e05`)
tout en conservant les spécificités DeepSeek : thinking mode, context caching,
tool calling parallèle, monitoring de contexte.

## Hypothèses retenues

- `README.md` sert de document maître opératif du repo.
- `PROTOCOLE.md` est la source canonique ; elle est chargée globalement via
  `~/.claude/CLAUDE.md` ou le champ `instructions` d'`opencode.json`.
- `show_context.ps1` lit les `.jsonl` opencode (même emplacement que Claude Code).
- DeepSeek V4 Pro = 1M tokens de contexte (source : api-docs.deepseek.com).
- La numérotation des versions reflète PROTOCOLE-CODEX pour faciliter la
  comparaison ; le saut de v1.0 à v1.4 est documenté dans `CHANGELOG.md`.
- L'installation peut être déléguée à l'IA par une demande en langage naturel
  avec l'URL du dépôt ; l'utilisateur n'a pas à manipuler les fichiers.
- Les limites de permissions et les arbitrages structurants restent explicites.
- `rtk` et `gh` sont des outils optionnels de workflow, pas des sources
  de vérité du projet.

## État courant

- 2026-09-13 : alignement v1.4 sur PROTOCOLE-CODEX v1.4, à la demande explicite
  de l'utilisateur. Ajouts : critères de réussite du chantier, preuves et portée
  de la validation, boucle de réalisation et de correction, intégrité des
  contrôles, installation déléguée, template `CLAUDE.md` projet, scénarios et
  journal de validation. Repo publié sur main.
  Critères du chantier : texte aligné sur la révision Codex, spécificités
  DeepSeek conservées, documents de suivi fidèles. Vérifications : Git, UTF-8,
  blocs Markdown. Essais comportementaux non exécutés.

- 2026-04-24 : v1.0 publiée — adaptation approfondie avec les spécificités
  DeepSeek (thinking mode, context caching disque, tool calling parallèle,
  rate limiting) et opencode (AGENTS.md, agents Build/Plan/General/Explore,
  skills, LSP, commandes slash, agent de compaction).

## Priorités hautes

Format : `[ ]` à faire, `[~]` partiellement fait, `[x]` fait.

### [~] HP1 — Maintenir l'alignement PROTOCOLE-CODEX

**Objectif** :
- Garder `PROTOCOLE.md`, `README.md`, `integrations/opencode.md` et
  `templates/` synchronisés avec les évolutions de PROTOCOLE-CODEX,
  sans perdre les spécificités DeepSeek.

**Actions** :
- Suivre les révisions de PROTOCOLE-CODEX et adapter chaque évolution.
- Vérifier que la hiérarchie des instructions reste claire pour un non-développeur.

**Livrables** :
- Protocole canonique, guide d'intégration, template `CLAUDE.md`, changelog.

**Résultats observables attendus** :
- Une session opencode chargée depuis ce repo applique les mêmes règles de fond
  qu'une session Codex chargée depuis PROTOCOLE-CODEX, aux surfaces près.

**Comportements à préserver** :
- Sections 12 et 13 spécifiques DeepSeek (monitoring, thinking, caching,
  tool calling) ; les parcours manuels d'installation.

**Vérifications prévues** :
- Comparaison section par section avec la révision Codex de référence ;
  contrôles UTF-8 et blocs Markdown ; `git diff --check`.

**Critère de fin** :
- Un utilisateur peut installer opencode en mode global, projet ou hybride
  sans ambiguïté sur `CLAUDE.md`, `AGENTS.md`, `opencode.json`, `rtk`, `gh`
  et les sources de vérité.

### [ ] HP2 — Compléter la validation comportementale de v1.4

**Objectif** : rendre les validations reproductibles et démontrer le chargement
du protocole dans une session neuve.

**Actions** :
- Exécuter les scénarios de `VALIDATION_SCENARIOS.md` dans un projet d'essai
  isolé, avec données fictives.
- Consigner chaque résultat, traiter les écarts, retester ce qui a changé.

**Livrables** :
- Journal de validation complété avec preuves des essais.

**Résultats observables attendus** :
- Le protocole chargé est effectivement appliqué : création du maître avant
  toute action, distinction repo / prod / validation, non-affaiblissement
  des contrôles.

**Comportements à préserver** :
- Aucune modification des permissions réelles ni des instructions globales
  pendant les essais.

**Vérifications prévues** :
- Les sept cas comportementaux et les six cas d'installation de
  `VALIDATION_SCENARIOS.md`, chacun avec preuve et statut réel.

**Critère de fin** :
- Preuves d'essais consignées, écarts traités, publication et installation
  explicitement renseignées. La relecture seule ne satisfait pas ce critère.

## Priorités moyennes

### [ ] MP1 — Tester l'installation sur un projet vierge

**Objectif** :
- Valider que le protocole est chargé correctement dans une session opencode
  neuve, sur un projet sans document maître.

**Actions** :
- Créer un projet test sans `DOCUMENT_MAITRE.md`.
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

### [x] BP1 — Ajouter un exemple de projet sensible

**Objectif** :
- Montrer comment documenter auth, données utilisateurs ou prod sensible
  dans un `CLAUDE.md` projet.

**Livrables** :
- Exemple de zone sensible et tableau « Contrôles par zone » intégrés à
  `templates/CLAUDE.md` (v1.4).

**Critère de fin** :
- Le template montre clairement comment cadrer une zone sensible.

## Ordre recommandé d'exécution

1. Vérifier l'intégration déléguée, y compris les personnalisations et réinstallations.
2. Exécuter les scénarios comportementaux isolés et consigner les résultats.
3. Traiter les écarts puis publier et installer dans les cibles autorisées.

## Definition of done pour le prochain cap

Le projet franchit un cap solide quand :
- l'installation globale, projet et hybride donne les mêmes règles ;
- les outils optionnels (`rtk`, `gh`) sont cadrés sans devenir des dépendances cachées ;
- le monitoring de contexte fonctionne sous Windows et est documenté.

## Prochaine action recommandée

Exécuter les scénarios de `VALIDATION_SCENARIOS.md` dans un projet isolé,
y compris le test initial sans document maître, puis consigner leurs résultats
avant de déclarer la v1.4 stable.
