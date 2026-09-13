# CLAUDE.md - [NOM DU PROJET]

Ce fichier connecte le projet au protocole DeepSeek/opencode. Il ne remplace pas
`PROTOCOLE.md` par une version courte.

## Spécificités projet

- **Nom** :
- **But** :
- **Langue de travail** : français sauf indication contraire
- **Stack** :
- **Cible de prod** :
- **Zones sensibles** : auth, données utilisateurs, paiements, migrations,
  sécurité, RGPD, secrets, ou autre zone à préciser

## Sources de vérité projet

À lire en début de session, dans cet ordre :

1. `DOCUMENT_MAITRE.md` ou `docs/DOCUMENT_MAITRE.md`
2. `ROADMAP.md` ou `docs/ROADMAP.md`
3. Dernier `VALIDATION_LOG.md` utile ou journal équivalent
4. Code, migrations, scripts et configuration réellement présents

Si le document maître ou la roadmap manque, suivre la création prévue par le
protocole, avec inspection factuelle limitée. Une intégration explicitement déléguée
inclut leur rédaction factuelle ; conserver les arbitrages pour les choix structurants.

## Commandes projet

- Installation :
- Développement local :
- Tests :
- Lint / format :
- Build :
- Déploiement :
- GitHub / forge :

## Contrôles par zone

Renseigner les commandes existantes après les avoir vérifiées dans le projet.
Ne pas inventer une commande pour remplir cette fiche. Déclarer les manques.

| Zone touchée | Comportement à préserver | Contrôle et environnement | Preuve attendue |
|---|---|---|---|
| ... | ... | ... | ... |

Exemple à adapter : pour l'authentification, vérifier qu'un compte A ne peut pas
lire les données du compte B, y compris par accès direct à l'API. Utiliser des
comptes et données de test autorisés, et noter le scénario et le refus observé.
Les commandes, URL et comptes restent à renseigner à partir du projet réel.

Les corrections ordinaires suivent la boucle du protocole dans le périmètre
convenu. Les contrôles obligatoires ne sont pas affaiblis pour obtenir un succès.

## `rtk`

`rtk` peut exister comme wrapper dans certains environnements. Ne pas l'utiliser
systématiquement. Si le projet le mentionne ou si une commande sensible le
justifie, vérifier d'abord qu'il existe (`Get-Command rtk`, `where rtk`,
`command -v rtk`). S'il n'existe pas, utiliser les commandes standard et le dire.

## `gh`

`gh` peut être utile si le projet passe souvent par GitHub (PR, issues,
checks CI, releases, API). Le préciser ici si c'est un outil attendu, sans en
faire une dépendance cachée si `git` + interface web suffisent.

Ne jamais documenter un token brut dans ce fichier. Pour l'automatisation,
préférer `GH_TOKEN` ou `GITHUB_TOKEN`.

## Protocole canonique

Si le protocole complet est déjà chargé globalement (`~/.claude/CLAUDE.md` ou
`opencode.json`), ne pas le répéter ici : garder ce fichier comme fiche projet.

Si ce projet doit être autonome, coller ci-dessous l'intégralité de :

```text
<chemin-vers-ce-repo>\PROTOCOLE.md
```

Ne pas coller une synthèse.
