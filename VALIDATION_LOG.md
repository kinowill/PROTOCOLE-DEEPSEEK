# Journal de validation — PROTOCOLE DEEPSEEK

## 2026-09-13 — Alignement v1.4 sur PROTOCOLE-CODEX et publication

- Demande explicite : adapter et améliorer le protocole DeepSeek à partir de
  PROTOCOLE-CODEX, s'aligner dessus, puis aligner le repo GitHub
  (https://github.com/kinowill/PROTOCOLE-DEEPSEEK).
- État testé : working tree basé sur le commit `0a70f3b` (v1.0) de `main`,
  avant publication. Environnement Windows / PowerShell, contrôles exécutés
  par l'agent avec Git, Python et ripgrep.
- Repo : publication effectuée sur `main` après contrôles. Production
  applicative : non applicable pour ce dépôt documentaire. Aucune release
  stable créée.
- Installation : le `opencode.json` global pointe déjà vers
  `C:\PROJETS\PROTOCOLE DEEPSEEK\PROTOCOLE.md` ; la v1.4 sera donc chargée
  aux prochaines sessions. Chargement en nouvelle session non observé depuis
  la session de travail.

| Contrôle exécuté | Attendu | Résultat observé | Statut |
|---|---|---|---|
| `git diff --check` | Aucun défaut d'espacement signalé | Aucune sortie, code 0 | réussi |
| Lecture UTF-8 des 10 fichiers markdown modifiés ou créés | Fichiers lisibles en UTF-8 | Tous décodés sans erreur | réussi |
| Équilibre des blocs de code Markdown | Nombre de clôtures ```` ``` ```` pair par fichier | Tous pairs | réussi |
| Correspondance des sections `##` avec PROTOCOLE-CODEX v1.4 | 15 sections, mêmes numéros, même ordre | 15 / 15 identiques (différences assumées : section 4 tiret cadratin, section 13 spécifique DeepSeek) | réussi |
| Taille de `PROTOCOLE.md` | Lisible, sans limite documentée côté cible | 32 197 octets | réussi |

- Reproduction des contrôles : lire chaque fichier en UTF-8, compter les
  lignes commençant par ```` ``` ````, comparer les titres `##` des deux
  protocoles. Référence Codex : révision
  `a24f2443f64b09932a27db7343a87f6b18b86e05`.
- Limites : contrôles documentaires uniquement. Les scénarios de
  `VALIDATION_SCENARIOS.md` sont préparés mais non exécutés ; le comportement
  dans une nouvelle session opencode, la reprise après compaction et les modes
  d'installation global/projet/hybride ne sont pas démontrés.
- Prochaine action : vérifier le chargement de la v1.4 dans une nouvelle
  session (le protocole global doit mentionner PROTOCOLE-DEEPSEEK v1.4), puis
  exécuter les scénarios isolés avant de déclarer la validation
  comportementale complète.

## Historique antérieur

Aucune entrée : la v1.0 (2026-04-24) n'a pas laissé de journal de validation
dans ce dépôt.
