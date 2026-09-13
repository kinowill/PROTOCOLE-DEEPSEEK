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

## État courant du dépôt

- Version de `main` : **v1.4**, alignée sur PROTOCOLE-CODEX v1.4
  (révision `a24f2443f64b09932a27db7343a87f6b18b86e05`) et adaptée à
  DeepSeek V4 Pro / opencode.
- Repo : https://github.com/kinowill/PROTOCOLE-DEEPSEEK.
- Validation documentaire : voir [VALIDATION_LOG.md](VALIDATION_LOG.md).
- Essais comportementaux : préparés dans [VALIDATION_SCENARIOS.md](VALIDATION_SCENARIOS.md),
  non exécutés à ce jour. Aucune release stable créée.

## Démarrage rapide : demander à DeepSeek / opencode

Dans la tâche du projet concerné, envoyer simplement :

> Regarde https://github.com/kinowill/PROTOCOLE-DEEPSEEK et intègre ce protocole
> dans mon projet. Préserve mes consignes existantes, crée la documentation
> factuelle manquante et vérifie l'installation.

L'IA lit le dépôt et fait l'intégration. Aucun copier-coller de fichier ni
remplissage de modèle n'est demandé à l'utilisateur. La procédure complète,
y compris pour un `CLAUDE.md` déjà présent, est dans
[integrations/opencode.md](integrations/opencode.md).

Pour tous les projets, préciser : « Installe-le globalement dans opencode ».
Pour le mode hybride, préciser : « Installe-le globalement et complète les
consignes de ce projet ». Le mode hybride conserve le protocole complet au
niveau global et les seules spécificités au niveau projet.

L'IA respecte les autorisations de l'environnement. Elle ne sollicite l'utilisateur
que pour une information indispensable, une décision structurante ou une permission
requise. Elle indique si une nouvelle session reste nécessaire pour confirmer le
chargement ; elle ne prétend pas avoir testé un comportement non observé.

## Installation manuelle (alternative pour les développeurs)

Le parcours manuel reste disponible. Avant de copier ou coller dans un fichier
existant, le lire, le sauvegarder et préserver ses consignes personnalisées.
Pour une mise à jour, suivre la procédure détaillée du guide d'intégration : ne
remplacer que l'ancien bloc canonique identifié, sans ajouter de doublon.

### Installation globale recommandée

À utiliser si tu veux qu'opencode applique le protocole dans tous tes projets.

1. Ouvre ou crée le fichier global opencode :

```text
Windows : C:\Users\<toi>\.claude\CLAUDE.md
macOS / Linux : ~/.claude/CLAUDE.md
```

2. Colle dedans **l'intégralité** de `PROTOCOLE.md`.

3. Relance opencode.

4. Teste dans un projet sans document maître : opencode doit proposer de créer le
   maître avant d'agir sur le chantier demandé.

Variante sans recopie : pointer le champ `instructions` de
`~/.config/opencode/opencode.json` vers le fichier canonique.

### Installation par projet

À utiliser si le protocole doit vivre dans un repo précis.

1. Copie `templates/CLAUDE.md` à la racine du projet.
2. Remplis les spécificités projet : stack, commandes, sources de vérité,
   zones sensibles, contraintes de prod.
3. Colle ensuite l'intégralité de `PROTOCOLE.md` dans la section prévue si le
   projet doit être autonome.

### Hybride recommandé

Le mode le plus pratique au quotidien :

- `~/.claude/CLAUDE.md` (ou `opencode.json`) charge le protocole complet ;
- chaque projet contient un `CLAUDE.md` court avec seulement ses spécificités ;
- le `CLAUDE.md` projet ne résume pas le protocole, il le complète.

## Ce que contient ce dossier

| Fichier | Rôle |
|---|---|
| `PROTOCOLE.md` | Le protocole canonique complet. C'est le cœur du dossier. |
| `ROADMAP.md` | Backlog opératif du repo protocole lui-même. |
| `CHANGELOG.md` | Historique des versions du protocole DeepSeek. |
| `VALIDATION_LOG.md` | Vérifications effectuées sur ce dépôt et limites. |
| `VALIDATION_SCENARIOS.md` | Scénarios à exécuter pour tester le protocole. |
| `show_context.ps1` | Outil d'auto-monitoring du contexte pour opencode/DeepSeek (section 12 du protocole). Lit les `.jsonl` de session opencode et affiche la consommation de tokens. **Windows / PowerShell uniquement.** |
| `templates/DOCUMENT_MAITRE.md` | Squelette de document maître à copier dans un nouveau projet. |
| `templates/ROADMAP.md` | Squelette de roadmap minimale. |
| `templates/VALIDATION_LOG.md` | Squelette de journal de validation. |
| `templates/CLAUDE.md` | Squelette `CLAUDE.md` pour connecter un projet à opencode sans résumer le protocole. |
| `integrations/opencode.md` | Méthode d'intégration opencode : global, projet, skills, agents, contexte, `rtk`, `gh`. |

## Responsabilités de l'IA pendant l'intégration

1. Lire le protocole complet et le guide depuis une même révision identifiée.
2. Identifier le projet cible, ses sources de vérité et ses instructions existantes.
3. Sauvegarder les fichiers concernés et intégrer le texte complet sans écraser
   les personnalisations. Une réinstallation identique ne crée aucun doublon.
4. Réutiliser les documents existants ; créer et renseigner les documents factuels
   manquants avec les éléments vérifiés du projet.
5. Vérifier les fichiers et rendre un bilan : version source, cible, sauvegarde,
   changements, contrôles réussis, chargement observé et limites.

## Validation de l'installation

L'IA vérifie directement l'intégrité du texte installé, les personnalisations,
les doublons, les overrides et la taille cumulée. Les scénarios comportementaux
sont décrits dans [VALIDATION_SCENARIOS.md](VALIDATION_SCENARIOS.md).
Dans un projet sans maître, l'inspection initiale doit permettre de documenter les
faits sans les inventer ; l'installation explicitement déléguée inclut la création
de cette documentation. Une décision produit non résolue reste à faire arbitrer.
Si la surface ne permet pas de vérifier le chargement dans une nouvelle session,
l'IA note « installation vérifiée, chargement en nouvelle session non vérifié ».
La présence d'un document maître seule ne prouve pas que les instructions ont été
chargées ni que tous les comportements du protocole sont respectés.

## Utilisation manuelle et maintenance du dossier

1. Lire `PROTOCOLE.md` en entier au moins une fois.
2. Suivre `integrations/opencode.md` pour choisir l'intégration : globale,
   projet, ou hybride.
3. Sur chaque projet, copier les fichiers de `templates/` utiles à la racine
   ou dans `docs/`, puis les remplir.
4. Si le projet n'a pas encore de document maître ou de roadmap, l'IA doit
   les créer en début de session avant d'agir sur le chantier demandé.
5. Faire évoluer ce dossier, pas des copies divergentes. Quand une règle
   change, la modifier ici, puis réinjecter le protocole dans opencode.

## Test minimal manuel

Dans un projet sans document maître, sans autorisation préalable de le créer :

1. Lance une nouvelle session opencode.
2. Pose une demande simple sur le projet.
3. opencode doit constater l'absence du maître et proposer de le créer avant de
   modifier ou analyser profondément le projet.

Si opencode répond directement à la demande sans mentionner les sources de vérité,
le protocole n'est pas chargé ou il est trop faible dans la hiérarchie des
instructions. Vérifie `~/.claude/CLAUDE.md`, `opencode.json`, le `CLAUDE.md`
projet et le dossier de lancement.

## Spécificités DeepSeek V4 Pro

| Caractéristique | Valeur |
|---|---|
| Modèle | DeepSeek V4 Pro |
| Contexte | 1 000 000 tokens |
| Sortie max | 384 000 tokens |
| Thinking mode | Activé par défaut, effort `max` pour agents de codage |
| Tool calls | Parallèles natifs |
| Context caching | Cache disque activé par défaut (préfixes), cache hit ~12x moins cher |
| Rate limit | Dynamique, HTTP 429, timeout 10 min |
| API | Format OpenAI / Anthropic |

## Spécificités opencode

| Caractéristique | Détail |
|---|---|
| Surfaces d'instructions | `CLAUDE.md` (compatibilité Claude), `AGENTS.md` (natif, via `/init`), `opencode.json` |
| Agents natifs | Build (outils complets), Plan (lecture seule), General (subagent), Explore (subagent read-only) |
| Skills | `.agents/skills/`, `.opencode/skills/`, `.claude/skills/` |
| LSP | Chargement automatique |
| Commandes utiles | `/init`, `/undo`, `/redo`, `/share`, `/connect` |
| Compaction | Agent automatique, déclenché à l'approche de la limite de contexte |

## Positionnement opencode

opencode dispose de mécanismes propres : `CLAUDE.md`, `AGENTS.md`,
`opencode.json`, skills, agents natifs, LSP, commandes slash et compaction.
Le protocole DeepSeek les utilise comme surfaces d'intégration, mais ne délègue
jamais la vérité du projet à ces mécanismes.

La vérité durable reste : document maître, roadmap, journal de validation,
code réellement présent, migrations et prod réellement alignée.

## Licence

Usage libre, personnel ou commercial. Pas de garantie. Améliorer et redistribuer
encouragé.
