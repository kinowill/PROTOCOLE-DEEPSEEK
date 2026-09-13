# Changelog

## v1.4 — 2026-09-13 — Alignement sur PROTOCOLE-CODEX v1.4

La numérotation saute de v1.0 à v1.4 pour refléter PROTOCOLE-CODEX
(révision `a24f2443f64b09932a27db7343a87f6b18b86e05`) et faciliter la
comparaison entre les deux protocoles. La v1.0 DeepSeek contenait déjà
l'équivalent des évolutions Codex v1.1 (auto-monitoring), v1.2 (adaptation
agent) et v1.3 (cadrage `gh`).

### Ajouts

- Critères de réussite du chantier (section 3) : résultats observables
  attendus, comportements à préserver, contrôles prévus, avant toute
  modification fonctionnelle.
- Preuves et portée de la validation (section 5) : état testé identifiable
  par commit ou instantané conservé, statuts réels, non-régression.
- Boucle de réalisation et de correction (section 9) : correction limitée au
  périmètre autorisé, diagnostic en cas de blocage.
- Intégrité des contrôles (section 11) : interdiction d'affaiblir un contrôle
  pour obtenir un résultat positif.
- Installation déléguée à DeepSeek (via opencode) : procédure complète avec
  sauvegarde, marqueurs `<!-- BEGIN PROTOCOLE-DEEPSEEK -->`, conservation des
  personnalisations, réinstallation sans doublon et compte rendu vérifié.
- Inspection initiale en lecture seule autorisée pour établir le document
  maître ; création factuelle déléguée lors d'une installation autorisée,
  sans retirer l'arbitrage des décisions structurantes.
- Section 12 enrichie : règle centrale d'écriture de l'état dans les sources
  de vérité avant compaction, reprise après compaction, moments de contrôle.
- `VALIDATION_SCENARIOS.md` : scénarios comportementaux et d'installation.
- `VALIDATION_LOG.md` : journal de validation du dépôt protocole.
- `templates/CLAUDE.md` : fiche projet opencode (équivalent du template
  `AGENTS.md` Codex), avec contrôles par zone et exemple de zone sensible.
- Templates maître / roadmap / journal mis à jour au format v1.4 (champs
  résultats attendus, état exact testé, publication et installation).

### Changements

- `integrations/opencode.md` : parcours manuels conservés, installation
  déléguée ajoutée comme option recommandée, variante `opencode.json`,
  maintenance du dépôt source et test minimal manuel.
- `README.md` : état courant, démarrage rapide délégué, responsabilités de
  l'IA pendant l'intégration, tableau des fichiers, positionnement opencode.
- `ROADMAP.md` : format v1.4 avec champs de critères de réussite.

### Validation

- Vérifications documentaires consignées dans VALIDATION_LOG.md.
- Essais comportementaux préparés mais non exécutés.
- Aucun changement des permissions, de la politique d'agents parallèles ou
  de l'exigence de conservation du protocole complet.

## v1.0 — 2026-04-24

Création du protocole DeepSeek, basé sur les versions Claude v1.2 et Codex v1.3,
avec adaptation approfondie aux spécificités DeepSeek V4 Pro et opencode.

### Contenu initial

- `PROTOCOLE.md` — protocole canonique en 14 sections, adapté spécifiquement pour :
  - **Thinking mode** : activé par défaut, effort `max` automatique pour opencode,
    `reasoning_content` conservé uniquement sur les tours avec tool calls, compté
    dans le budget de 1M tokens.
  - **Context caching** : cache disque activé par défaut, ~12x moins cher en cache
    hit. Le protocole de début de session (lecture ordonnée des sources de vérité)
    crée un préfixe stable qui maximise les cache hits automatiquement.
  - **Tool calling** : parallèle natif, à utiliser pour les lectures indépendantes.
  - **Rate limit** : HTTP 429 dynamique, timeout 10 min sur requêtes en attente.
- `integrations/opencode.md` — guide complet d'intégration couvrant :
  - `CLAUDE.md` et `AGENTS.md` (double surface)
  - Agents natifs : Build, Plan, General, Explore
  - Skills (6 chemins de découverte)
  - Context caching et thinking mode côté opencode
  - Commandes slash (`/init`, `/undo`, `/redo`, `/share`)
  - Auto-monitoring avec `show_context.ps1`
- `show_context.ps1` — outil PowerShell, fenêtre 1M tokens, lit `~/.claude/projects/`
- `templates/` — squelettes standard : document maître, roadmap, journal de validation
- `README.md`, `ROADMAP.md`, `CHANGELOG.md`, `.gitignore`, `LICENSE`

### Sources

- Documentation officielle DeepSeek API (api-docs.deepseek.com) :
  Thinking Mode, Tool Calls, Context Caching, Rate Limit, Coding Agents
- Documentation officielle opencode (opencode.ai/docs) :
  Agents, Skills, Config
- `PROTOCOLE CLAUDE` v1.2 et `PROTOCOLE CODEX` v1.3
