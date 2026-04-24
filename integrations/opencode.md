# Intégration — opencode

## Principe

opencode supporte deux surfaces d'instructions persistantes :

- **`CLAUDE.md`** (compatibilité Claude Code) : lu automatiquement à deux
  niveaux — global (`~/.claude/CLAUDE.md`) et projet (`CLAUDE.md` à la racine).
- **`AGENTS.md`** (natif opencode) : généré par `/init`, documente la stack,
  les commandes et la structure du projet.

Le protocole canonique vit dans `~/.claude/CLAUDE.md`. Le `AGENTS.md` projet
complète, il ne remplace pas.

## Surfaces opencode concernées

| Surface | Rôle dans le protocole |
|---|---|
| `~/.claude/CLAUDE.md` | Instructions globales chargées dans tous les projets (support principal du protocole). |
| `CLAUDE.md` projet | Instructions versionnées avec un repo précis. |
| `AGENTS.md` projet | Généré par `/init`, fiche projet native opencode. |
| Agents (Build/Plan/General/Explore) | Modes de travail : Build = outils complets, Plan = lecture seule. |
| Skills (`~/.agents/skills/`, `.opencode/skills/`) | Capacités ciblées, chargées à la demande via l'outil `skill`. |
| `/init`, `/undo`, `/redo`, `/share` | Commandes slash de session.

## Option A — Protocole global opencode

À utiliser si tu veux que toutes tes sessions opencode héritent du protocole.

1. Ouvrir ou créer :

```text
C:\Users\<toi>\.claude\CLAUDE.md
```

2. Coller **l'intégralité** de `PROTOCOLE.md` dans ce fichier.

3. Ne pas coller une synthèse. Si la taille devient un jour un problème,
   déplacer les compléments dans des `CLAUDE.md` plus proches des sous-dossiers,
   mais ne pas réduire le protocole sans arbitrage explicite.

4. Relancer opencode.

5. Vérifier que le protocole est actif en demandant à opencode, dans un projet
   sans document maître, de commencer une session. Il doit identifier l'absence
   de maître et proposer de le créer avant d'agir sur le chantier demandé.

## Option B — Protocole par projet

À utiliser si tu veux versionner le protocole avec un repo précis.

1. Copier `PROTOCOLE.md` dans le `CLAUDE.md` à la racine du projet.
2. Ajouter en haut une section spécifique au projet :

```markdown
# CLAUDE.md — [NOM DU PROJET]

## Spécificités projet

- **Langue** : français
- **Stack** : (résumé)
- **Contraintes** : utilisateur non-développeur, prod sensible, etc.
- **Sources de vérité principales** :
  - docs/DOCUMENT_MAITRE.md
  - docs/ROADMAP.md
  - docs/VALIDATION_LOG.md

---

(ici tu colles le contenu de PROTOCOLE.md)
```

## Option C — Hybride recommandé

Pour un usage quotidien, le plus robuste est :

- `~/.claude/CLAUDE.md` contient l'intégralité du protocole ;
- chaque projet contient un `CLAUDE.md` court qui ne répète pas le protocole,
  mais ajoute les spécificités du projet ;
- si un projet nécessite une règle contradictoire, l'utilisateur arbitre et la
  contradiction est documentée dans le `CLAUDE.md` projet.

Dans cette option, le `CLAUDE.md` projet n'est pas une synthèse du protocole.
C'est une fiche projet complémentaire.

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

Utilisation correcte :
- créer une skill pour un workflow spécialisé ;
- annoncer quand elle est utilisée ;
- garder `CLAUDE.md` comme support principal du protocole ;
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

### Étape 2 — Coller ce bloc dans `~/.claude/CLAUDE.md`

Ajoute ce bloc à la fin de ton `CLAUDE.md` global. **Remplace
`<CHEMIN_VERS_PROTOCOLE_DEEPSEEK>`** par ton chemin réel.

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
que le `CLAUDE.md` est bien lu en début de session.

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

Avant compaction, fork ou changement d'agent, l'état réel doit être écrit
dans le document maître, la roadmap ou le journal de validation si la vérité
du projet a changé.

En cas de compaction, **relire les sources de vérité avant de continuer**
à modifier le code. Ne pas se fier au souvenir de session : la compaction
peut avoir supprimé des détails importants.

La fenêtre de 1M tokens de DeepSeek donne une grande marge, mais le
`reasoning_content` des tours avec tool calls s'accumule et peut accélérer
l'approche de la limite. Surveiller régulièrement avec `show_context.ps1`.

## Test minimal

Dans un projet sans document maître :

1. Lancer une nouvelle session opencode.
2. Poser une demande simple sur le projet.
3. opencode doit constater l'absence du maître et proposer de le créer avant
   de modifier ou analyser profondément le projet.

Si opencode répond directement à la demande sans mentionner les sources de vérité,
le protocole n'est pas chargé ou il est trop faible dans la hiérarchie des
instructions. Vérifier `~/.claude/CLAUDE.md` et le `CLAUDE.md` projet.
