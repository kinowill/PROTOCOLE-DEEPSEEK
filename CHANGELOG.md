# Changelog

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
