# Document maître - [NOM DU PROJET]

> Ce document est la référence opérative principale du projet.
> Il doit toujours pouvoir être lu seul et donner une image exacte du projet.
> Il est mis à jour à chaque session qui modifie la vérité du projet.

Dernière mise à jour : YYYY-MM-DD

---

## 1. Identité du projet

- **Nom** :
- **But en une phrase** :
- **Public visé** :
- **Statut** : (prototype / alpha / beta / production / maintenance)

## 2. Stack technique

- **Front** :
- **Back / serverless** :
- **Base de données** :
- **Hébergement** :
- **Auth** :
- **Autres services** :

## 3. Structure des dossiers

```
projet/
├── ...
└── ...
```

Règles :
- (où vit le code)
- (où vit la doc)
- (où vivent les migrations)
- (où vivent les tests)

## 4. État courant

- **Ce qui est stable** :
- **Ce qui est en cours** :
- **Ce qui est cassé ou douteux** :

## 5. Sources de vérité

| Niveau | Document | Rôle |
|---|---|---|
| Runtime actuelle | code + migrations déployées + env vars | la vérité ultime |
| Reprise technique | ce document + ROADMAP.md + dernier journal de validation | comment reprendre le projet |
| Conception long terme | (docs de conception, historique des décisions) | pourquoi le projet est comme il est |

## 6. Décisions structurantes

(Liste des choix techniques ou produits qu'il ne faut pas reprendre sans
arbitrage explicite : ex. "pas de framework JS", "Cloudflare Pages comme
seule cible", "auth uniquement par magic link", etc.)

## 7. Variables d'environnement et secrets

- (lister ce qui est attendu en env, sans valeurs)
- (lister où sont stockés les secrets)

## 8. Notes de maintenance

- (rappels utiles pour la prochaine session)
