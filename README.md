# Protocole de travail IA - DeepSeek

## Pourquoi ce protocole existe

Ce dossier contient un protocole de travail pour DeepSeek V4 Pro (via opencode)
appliqué à des projets maîtrisés par un utilisateur qui ne code pas forcément
lui-même.

L'enjeu n'est pas la productivité brute. L'enjeu est que **l'utilisateur reste
maître d'un projet qu'il ne code pas lui-même**. Sans cadre, un agent IA :

- avance sans laisser de trace lisible ;
- introduit des erreurs qui ne se voient que des semaines plus tard ;
- présente comme « fini » du code qui n'a jamais touché la prod ;
- repart à chaque session d'une interprétation différente du projet ;
- transforme progressivement le repo en boîte noire.

Le protocole transforme ce risque en processus contrôlable.

## Règle de lecture

`PROTOCOLE.md` est la source canonique. Il ne doit pas être remplacé par une
version courte, une checklist ou une synthèse. Les fichiers d'intégration
expliquent comment l'injecter dans opencode, mais ils ne remplacent pas sa
lecture complète.

## Démarrage rapide

### Installation globale recommandée

À utiliser si tu veux qu'opencode applique le protocole dans tous tes projets.

1. Ouvrir ou créer le fichier global opencode :

```text
Windows : C:\Users\<toi>\.claude\CLAUDE.md
```

2. Coller dedans **l'intégralité** de `PROTOCOLE.md`.

3. Relancer opencode.

4. Vérifier que le protocole est actif en demandant à opencode, dans un projet
   sans document maître, de commencer une session. Il doit identifier l'absence
   de maître et proposer de le créer avant d'agir sur le chantier demandé.

### Installation par projet

À utiliser si le protocole doit vivre dans un repo précis.

1. Copier `PROTOCOLE.md` à la racine du projet (ou le coller dans le `CLAUDE.md`
   projet).

2. Ajouter en haut les spécificités projet : stack, commandes, sources de vérité,
   zones sensibles, contraintes de prod.

### Hybride recommandé

Le mode le plus pratique au quotidien :

- `~/.claude/CLAUDE.md` contient le protocole complet ;
- chaque projet contient un `CLAUDE.md` court avec seulement ses spécificités ;
- le `CLAUDE.md` projet ne résume pas le protocole, il le complète.

## Ce que contient ce dossier

| Fichier | Rôle |
|---|---|
| `PROTOCOLE.md` | Le protocole canonique complet. C'est le cœur du dossier. |
| `ROADMAP.md` | Backlog opératif du repo protocole lui-même. |
| `CHANGELOG.md` | Historique des versions du protocole DeepSeek. |
| `show_context.ps1` | Outil d'auto-monitoring du contexte pour opencode/DeepSeek (section 12 du protocole). Lit les `.jsonl` de session opencode et affiche la consommation de tokens. **Windows / PowerShell uniquement.** |
| `templates/DOCUMENT_MAITRE.md` | Squelette de document maître à copier dans un nouveau projet. |
| `templates/ROADMAP.md` | Squelette de roadmap minimale. |
| `templates/VALIDATION_LOG.md` | Squelette de journal de validation. |
| `integrations/opencode.md` | Guide d'injection du protocole dans opencode : `CLAUDE.md`, `AGENTS.md`, skills, agents, contexte, `rtk`, `gh`. |

## Comment utiliser ce dossier

1. Lire `PROTOCOLE.md` en entier au moins une fois.
2. Suivre `integrations/opencode.md` pour choisir l'intégration : globale,
   projet, ou hybride.
3. Sur chaque projet, copier les fichiers de `templates/` utiles à la racine
   ou dans `docs/`, puis les remplir.
4. Activer l'auto-monitoring du contexte avec `show_context.ps1` (section 12).
5. Faire évoluer ce dossier, pas des copies divergentes. Quand une règle
   change, la modifier ici, puis réinjecter le protocole dans opencode.

## Test minimal

Dans un projet sans document maître :

1. Lancer une nouvelle session opencode.
2. Poser une demande simple sur le projet.
3. opencode doit constater l'absence du maître et proposer de le créer avant
   de modifier ou analyser profondément le projet.

Si opencode répond directement à la demande sans mentionner les sources de vérité,
le protocole n'est pas chargé ou il est trop faible dans la hiérarchie des
instructions. Vérifier `~/.claude/CLAUDE.md` et le `CLAUDE.md` projet.

## Spécificités DeepSeek V4 Pro

| Caractéristique | Valeur |
|---|---|
| Modèle | DeepSeek V4 Pro |
| Contexte | 1 000 000 tokens |
| Sortie max | 384 000 tokens |
| Thinking mode | Activé par défaut, effort `max` pour agents de codage |
| Tool calls | Parallèles natifs, mode strict (beta) disponible |
| Context caching | Cache disque activé par défaut (préfixes), cache hit ~12x moins cher |
| Rate limit | Dynamique, HTTP 429, timeout 10 min |
| API | Format OpenAI / Anthropic |

## Spécificités opencode

| Caractéristique | Détail |
|---|---|
| Surfaces d'instructions | `CLAUDE.md` (compatibilité Claude), `AGENTS.md` (natif, via `/init`) |
| Agents natifs | Build (outils complets), Plan (lecture seule), General (subagent), Explore (subagent read-only) |
| Skills | `.agents/skills/`, `.opencode/skills/`, `.claude/skills/` |
| LSP | Chargement automatique |
| Commandes utiles | `/init`, `/undo`, `/redo`, `/share`, `/connect` |
| Compaction | Agent automatique, déclenché à l'approche de la limite de contexte |

## Licence

Usage libre, personnel ou commercial. Pas de garantie. Améliorer et redistribuer
encouragé.
