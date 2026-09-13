# Intégration — opencode

## Principe

opencode lit des instructions persistantes via plusieurs surfaces :
`CLAUDE.md` (compatibilité Claude Code), `AGENTS.md` (natif opencode) et le
champ `instructions` d'`opencode.json`. C'est la surface naturelle pour
appliquer ce protocole.

Le protocole ne doit pas être injecté sous forme courte. `PROTOCOLE.md` est le
texte canonique complet. Les instructions ci-dessous servent uniquement à le
charger correctement dans opencode.

## Surfaces opencode concernées

| Surface | Rôle dans le protocole |
|---|---|
| `~/.claude/CLAUDE.md` | Instructions globales chargées dans tous les projets. Support principal du protocole. |
| `CLAUDE.md` projet | Instructions versionnées avec un repo précis. |
| `AGENTS.md` projet | Format natif opencode, généré par `/init` ; fiche projet. |
| `opencode.json` | Champ `instructions` : liste de fichiers chargés dans la session (global ou projet). |
| Agents (Build/Plan/General/Explore) | Modes de travail : Build = outils complets, Plan = lecture seule. |
| Skills | Workflows réutilisables, utiles mais non garantis à chaque session. |
| Commandes slash | Contrôle de session : `/init`, `/undo`, `/redo`, `/share`, `/connect`. |
| Compaction | Agent automatique déclenché à l'approche de la limite de contexte. |

## Installation et mise à jour exécutées par DeepSeek (via opencode)

La demande « regarde ce dépôt et intègre le protocole » est une tâche à réaliser,
pas une demande de tutoriel. L'utilisateur n'a pas à copier le texte, créer les
fichiers, remplir les modèles ou exécuter les contrôles à la place de l'IA.
Une demande de simple lecture ou d'avis n'autorise pas l'installation.

### Cible et source

- Respecter la portée demandée : projet, globale ou hybride. Sans précision,
  utiliser le projet de travail identifié ; si absent ou ambigu, demander la cible.
- Projet : intégrer dans son `CLAUDE.md` sans écraser son contenu existant.
- Globale : déterminer la surface réellement chargée (`~/.claude/CLAUDE.md` ou
  `opencode.json` global) ; ne pas déduire un chemin d'un exemple.
- Hybride : protocole complet global, spécificités factuelles dans le projet.
- Lire README, PROTOCOLE.md, roadmap et ce guide depuis une même révision source.
  Consigner l'URL, la révision et la version. Signaler une version candidate ; ne
  pas la présenter comme une version validée. Ne pas modifier le dépôt source
  simplement parce que l'utilisateur souhaite l'utiliser dans un autre projet.

### Inspection et préparation

1. Lire les instructions globales et de projet pertinentes, les overrides et les
   sources de vérité existantes. Vérifier les contraintes d'accès et l'état Git.
2. Avant une création de maître, effectuer l'inspection factuelle limitée prévue
   par le protocole. Réutiliser les documents équivalents déjà présents.
3. Repérer les personnalisations et l'ancien protocole. Comparer au texte source
   de la version installée si nécessaire ; ne pas utiliser une découpe approximative.
4. Si un `opencode.json` ou un `CLAUDE.md` masque l'autre surface, le préserver et
   établir une intégration dans le fichier effectivement chargé, sans duplication
   des consignes. Si cela exige de trancher une contradiction de fond, présenter
   seulement ce choix à l'utilisateur.
5. Sauvegarder hors des chemins d'instructions actives les fichiers avant changement.
   Conserver leurs octets, nommer la sauvegarde sans collision et noter son emplacement.
   Les sauvegardes contenant des informations privées restent locales.

### Intégration sans perte

6. Encadrer le protocole complet par les marqueurs suivants, avec sa provenance
   immédiatement au-dessus. Les marqueurs ne remplacent aucune partie du texte :

```text
Source : URL du dépôt, révision exacte, version
<!-- BEGIN PROTOCOLE-DEEPSEEK -->
[contenu intégral de PROTOCOLE.md]
<!-- END PROTOCOLE-DEEPSEEK -->
```

7. Première installation : insérer un seul bloc et préserver les autres consignes.
   Mise à jour : remplacer uniquement le bloc existant identifié. Sans marqueurs,
   retrouver ses limites par comparaison avant toute substitution ; si elles ne
   peuvent être établies sans risque, demander une clarification ciblée.
8. Réinstallation identique : conserver le bloc existant, sans duplication ni
   réécriture inutile. Préserver les personnalisations dans et hors du bloc : une
   divergence interne est à réconcilier explicitement, jamais à effacer en silence.
9. Créer et remplir uniquement les documents factuels manquants avec des informations
   vérifiées ; noter les inconnues. Les choix structurants non autorisés nécessitent
   un arbitrage, pas une invention. Ne pas copier des modèles vides sur des documents.

### Vérification et compte rendu

10. Relire les fichiers écrits et comparer le bloc à la source complète. Vérifier
    l'absence de doublons et la conservation des personnalisations. Contrôler le diff
    Git s'il existe ; pour les autres cibles, comparer à la sauvegarde.
11. Vérifier les instructions effectivement sélectionnées et la taille cumulée par
    rapport aux limites de la surface utilisée. Ne pas tronquer le protocole ni
    augmenter silencieusement une portée ou une permission.
12. Conserver un état identifiable des fichiers installés et un résultat de contrôle.
    Distinguer installation vérifiée, chargement observé et comportement testé.
    Ne pas lancer un agent supplémentaire sans autorisation explicite. Si une nouvelle
    session est nécessaire et ne peut pas être vérifiée par les outils autorisés,
    signaler cette limite et la seule action utilisateur encore nécessaire.
13. Rendre un bilan court : cible, version/révision, fichiers concernés, sauvegarde,
    contrôles et limites. Ne pas committer, pousser ou modifier la configuration
    globale au-delà de la portée de la demande d'installation.

En cas de restauration, comparer d'abord l'état actuel à celui écrit pendant
l'installation : préserver les changements utilisateur intervenus entre-temps.
Si le fichier n'a pas changé, restaurer la sauvegarde exacte ; sinon appliquer
un retour ciblé vérifié. Aucun reset destructif ni écrasement aveugle.

## Parcours manuel conservé

Les options A, B et C ci-dessous sont destinées à qui souhaite effectuer lui-même
l'installation. Les mêmes garanties s'appliquent : lire et sauvegarder tout fichier
existant, préserver les personnalisations, identifier l'ancien bloc et éviter les
doublons. Copier un modèle ne doit jamais écraser un `CLAUDE.md` déjà renseigné.
En cas de mise à jour, appliquer les précautions de la procédure ci-dessus avant
le collage. Le parcours délégué reste une alternative, pas un remplacement.

## Option A — Protocole global opencode

À utiliser si tu veux que toutes tes sessions opencode héritent du protocole.

1. Ouvrir ou créer :

```text
C:\Users\<toi>\.claude\CLAUDE.md
```

Sous macOS / Linux :

```text
~/.claude/CLAUDE.md
```

2. Coller **l'intégralité** de `PROTOCOLE.md` dans ce fichier.

3. Ne pas coller une synthèse. Si la taille devient un jour un problème,
   déplacer les compléments dans des `CLAUDE.md` plus proches des sous-dossiers,
   mais ne pas réduire le protocole sans arbitrage explicite.

4. Relancer opencode.

5. Vérifier que le protocole est actif en demandant à opencode, dans un projet
   sans document maître, de commencer une session. Il doit identifier l'absence
   de maître et proposer de le créer avant d'agir sur le chantier demandé.

**Variante `opencode.json` :** au lieu de copier le texte, le champ
`instructions` peut référencer directement le fichier canonique :

```json
{
  "instructions": [
    "C:/PROJETS/PROTOCOLE DEEPSEEK/PROTOCOLE.md"
  ]
}
```

Le fichier source reste unique : la mise à jour du protocole profite alors à
toutes les sessions sans recopie. Si le chemin change, mettre à jour la
configuration en même temps que le protocole.

## Option B — Protocole par projet

À utiliser si tu veux versionner le protocole avec un repo précis.

1. Copier `templates/CLAUDE.md` à la racine du projet :

```text
<projet>\CLAUDE.md
```

2. Remplir les spécificités projet : stack, commandes, sources de vérité,
   zones sensibles, contraintes de prod.

3. Coller ensuite **l'intégralité** de `PROTOCOLE.md` dans la section prévue.

4. Committer `CLAUDE.md` avec le projet si le protocole doit suivre le repo.

## Option C — Hybride recommandé

Pour un usage quotidien, le plus robuste est :

- `~/.claude/CLAUDE.md` (ou `opencode.json`) charge l'intégralité du protocole ;
- chaque projet contient un `CLAUDE.md` court qui ne répète pas le protocole,
  mais ajoute les spécificités du projet ;
- si un projet nécessite une règle contradictoire, l'utilisateur arbitre et la
  contradiction est documentée dans le `CLAUDE.md` projet.

Dans cette option, le `CLAUDE.md` projet n'est pas une synthèse du protocole.
C'est une fiche projet complémentaire. Le `AGENTS.md` généré par `/init` documente
la stack, les commandes et la structure du projet.

## Agents opencode

opencode fournit des agents natifs. L'utilisateur bascule entre les agents
primaires avec **Tab**.

| Agent | Mode | Usage |
|---|---|---|
| **Build** | Primaire | Développement complet. Tous les outils activés. |
| **Plan** | Primaire | Analyse et planification. Pas d'écriture, pas de bash. |
| **General** | Subagent | Tâches complexes multi-étapes. Outils complets (sauf todo). |
| **Explore** | Subagent | Exploration rapide de codebase. Lecture seule. |

**Mode Plan :** l'utilisateur peut basculer en mode Plan pour faire analyser
une demande sans risque de modification. L'IA doit alors proposer un plan,
puis suggérer de passer en mode Build pour l'exécution.

Les subagents ne doivent pas être lancés automatiquement. Les utiliser seulement
si l'utilisateur le demande explicitement ou demande une délégation parallèle.

## Contexte DeepSeek et context caching

DeepSeek V4 Pro active par défaut un **cache disque** sur les préfixes de
contexte. Quand deux requêtes partagent le même début (ex. messages system +
lecture du document maître), la partie commune est servie depuis le cache.

**Cache hit :** 0,145 $/M tokens — **Cache miss :** 1,74 $/M tokens (~12x).

Ce mécanisme renforce naturellement le protocole : lire le document maître,
la roadmap et le journal de validation **dans le même ordre à chaque session**
crée un préfixe stable qui maximise les cache hits.

**Thinking mode :** activé par défaut, effort `max` automatique pour opencode.
Les tokens de thinking comptent dans le budget de 1M tokens. Les sessions avec
beaucoup de tool calls ont une pression de contexte plus élevée car le
`reasoning_content` des tours avec tool call est conservé.

## Skills opencode

Les skills opencode sont des instructions réutilisables au format `SKILL.md`,
chargées à la demande via l'outil `skill`. Chemins de découverte :

- `.opencode/skills/<nom>/SKILL.md`
- `~/.config/opencode/skills/<nom>/SKILL.md`
- `.claude/skills/<nom>/SKILL.md` (compatibilité Claude)
- `~/.claude/skills/<nom>/SKILL.md`
- `.agents/skills/<nom>/SKILL.md`
- `~/.agents/skills/<nom>/SKILL.md`

Une skill peut compléter le protocole, mais ne doit pas être son seul support.
opencode charge les skills par invocation explicite ou par correspondance avec
leur description. Le protocole, lui, doit être actif avant de décider quelles
skills sont utiles.

Utilisation correcte :
- créer une skill pour un workflow spécialisé ;
- annoncer quand elle est utilisée ;
- garder `CLAUDE.md` / `opencode.json` comme support principal du protocole ;
- ne pas remplacer la lecture des sources de vérité par une skill.

## `rtk`

`rtk` (Rust Token Killer) est un wrapper de commandes disponible dans
l'environnement opencode. Il réduit la verbosité des sorties de `git`,
`npm`, `cargo`, `tsc`, `lint`, etc.

Règles d'usage :
- préfixer systématiquement les commandes courantes par `rtk`
  (ex. `rtk git status`, `rtk cargo test`, `rtk npm run build`) ;
- même dans les chaînes avec `&&`, utiliser `rtk` ;
- si `rtk` n'est pas disponible, utiliser les commandes standard et le signaler ;
- ne jamais inventer une sous-commande `rtk`.

## GitHub CLI (`gh`)

`gh` est utile quand l'action vise GitHub directement : PR, issues, checks,
releases ou appels API GitHub. Le protocole demande de le traiter comme un
outil de workflow, jamais comme une source de vérité du projet.

Règles :
- vérifier qu'il est disponible (`Get-Command gh`, `where gh`, `command -v gh`) ;
- vérifier l'auth si nécessaire (`gh auth status`) ;
- si `gh` est absent ou non configuré, retomber sur `git`, l'interface web
  ou l'API déjà utilisée par le projet, et le dire ;
- ne jamais écrire un token brut dans `CLAUDE.md`, un script versionné ou
  une commande partagée ;
- pour l'automatisation, préférer `GH_TOKEN` ou `GITHUB_TOKEN`.

## Activer l'auto-monitoring du contexte (`show_context.ps1`)

Le protocole (section 12) prévoit que l'IA s'auto-vérifie sur sa
consommation de tokens avant toute grosse tâche. opencode/DeepSeek n'ont
**aucune introspection native** sur ce point : sans outil externe, l'IA
ne peut pas savoir où elle en est dans sa fenêtre de contexte de 1M tokens.

L'outil `show_context.ps1` (à la racine de ce dossier) lit les fichiers
`.jsonl` de session opencode (dans `~/.claude/projects/`, même emplacement
que Claude Code) et retourne, pour la session en cours, le total prompt
et son pourcentage de la fenêtre.

> **Notes importantes :**
> - Le chiffre correspond au **tour précédent** (le tour courant n'est pas
>   encore flushé). Prévoir une marge.
> - Les tokens de **thinking** (chaîne de raisonnement) sont inclus dans le
>   total prompt affiché.
> - Les tokens servis depuis le **cache disque** DeepSeek (cache hits) ne
>   sont pas comptés dans le total prompt mais occupent la fenêtre de contexte.
>   Un pourcentage bas ne garantit pas que la fenêtre est vide.
> - **Limite :** Windows / PowerShell uniquement. Portage macOS / Linux à faire
>   (les `.jsonl` opencode se trouvent dans `~/.claude/projects/<projet>/`).

### Étape 1 — Repérer le chemin local du dossier

Note l'emplacement où se trouve ce dossier, par exemple :
- Windows : `C:\PROJETS\PROTOCOLE DEEPSEEK`

### Étape 2 — Coller ce bloc dans le support global

Ajoute ce bloc à la fin de ton `CLAUDE.md` global ou de tes instructions
globales opencode. **Remplace `<CHEMIN_VERS_PROTOCOLE_DEEPSEEK>`** par ton
chemin réel.

```markdown
## Auto-monitoring du contexte

L'IA n'a aucune introspection native sur sa consommation de tokens. Avant toute
grosse tâche (refactor multi-fichiers, audit large, gros docs, batch d'edits) et
périodiquement sur les longues sessions, lancer via Bash :
`powershell.exe -NoProfile -ExecutionPolicy Bypass -File "<CHEMIN_VERS_PROTOCOLE_DEEPSEEK>/show_context.ps1"`

Le script affiche le % de la fenêtre 1M consommé. Seuils d'action :
- **< 50 %** feu vert.
- **50–75 %** prévenir, penser à découper.
- **75–90 %** prévenir, **sauvegarder l'état** (commit WIP, note dans journal, maître à jour), découper.
- **> 90 %** stop, tout sauvegarder dans maître/journal, proposer une nouvelle session.

Le chiffre lu = état au **tour précédent** (le tour courant n'est pas encore flushé), prévoir une marge.
Détails complets : section 12 de `PROTOCOLE.md`.
```

### Étape 3 — Vérifier que l'auto-monitoring est actif

Dans une nouvelle session opencode, demande simplement :
> « Combien de tokens reste-t-il dans ma fenêtre de contexte ? »

Si opencode répond en exécutant la commande `powershell.exe ... show_context.ps1`
et te donne un pourcentage réel, c'est gagné. S'il répond « je ne sais pas »
ou s'il invente, le bloc n'est pas chargé : vérifier le chemin et le fait
que le support global est bien lu en début de session.

### Côté utilisateur (toi, pas l'IA)

Pour pouvoir consulter l'état du contexte en deux clics, créer un raccourci
`.bat` sur le bureau qui contient :

```bat
@echo off
chcp 65001 >nul
title DeepSeek / opencode — Etat des contextes
powershell.exe -NoProfile -ExecutionPolicy Bypass -File "<CHEMIN_VERS_PROTOCOLE_DEEPSEEK>\show_context.ps1"
echo.
pause
```

## Contexte opencode

opencode déclenche automatiquement son agent de **compaction** quand le
contexte approche la limite. La compaction compresse l'historique en un
résumé et libère de l'espace.

Avant compaction ou changement d'agent, l'état réel doit être écrit
dans le document maître, la roadmap ou le journal de validation si la vérité
du projet a changé.

En cas de compaction, **relire les sources de vérité avant de continuer**
à modifier le code. Ne pas se fier au souvenir de session : la compaction
peut avoir supprimé des détails importants.

La fenêtre de 1M tokens de DeepSeek donne une grande marge, mais le
`reasoning_content` des tours avec tool calls s'accumule et peut accélérer
l'approche de la limite. Surveiller régulièrement avec `show_context.ps1`.

## Maintenance et validation du dépôt source

Pour faire évoluer ce protocole, travailler sur une branche dédiée, relire les
changements avec les modèles et exécuter les scénarios de
`../VALIDATION_SCENARIOS.md`. Consigner les essais réellement exécutés et les limites.
Une publication sur GitHub ne met pas à jour les installations existantes : l'IA
applique ensuite la procédure d'intégration ci-dessus à chaque cible autorisée.
Un retour sur une version publiée utilise un commit correctif ou un revert ciblé,
sans réécrire l'historique partagé.

## Test minimal manuel

Dans un projet sans document maître, sans autorisation préalable de le créer :

1. Lancer une nouvelle session opencode.
2. Poser une demande simple sur le projet.
3. opencode doit constater l'absence du maître et proposer de le créer avant de
   modifier ou analyser profondément le projet.

Si opencode répond directement à la demande sans mentionner les sources de vérité,
le protocole n'est pas chargé ou il est trop faible dans la hiérarchie des
instructions. Vérifier `~/.claude/CLAUDE.md`, `opencode.json`, le `CLAUDE.md`
projet et le dossier de lancement.
