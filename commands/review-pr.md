---
description: Revue complète d'une PR avec 6 agents spécialisés (commentaires, tests, erreurs, types, qualité, simplification)
argument-hint: "[comments] [tests] [errors] [types] [code] [simplify] [all] [parallel]"
---

# Revue Complète de PR

Lance une revue de pull request en utilisant plusieurs agents spécialisés, chacun se concentrant sur un aspect différent de la qualité du code.

**Aspects de revue demandés :** "$ARGUMENTS"

## Workflow de revue

### 1. Identifier la portée

- Vérifier les fichiers modifiés : !`git diff --name-only HEAD`
- Diff complet : !`git diff HEAD`
- Analyser les arguments pour voir si des aspects spécifiques sont demandés
- Par défaut : lancer toutes les revues applicables

### 2. Aspects disponibles

- **comments** — Analyser la précision et la maintenabilité des commentaires de code
- **tests** — Évaluer la qualité et la complétude de la couverture de tests
- **errors** — Vérifier la gestion des erreurs et les échecs silencieux
- **types** — Analyser la conception des types et les invariants (si de nouveaux types ont été ajoutés)
- **code** — Revue générale du code selon les conventions du projet
- **simplify** — Simplifier le code pour plus de clarté et de maintenabilité
- **all** — Lancer toutes les revues applicables (par défaut)

### 3. Déterminer les revues applicables

Selon les changements :
- **Toujours applicable** : code-reviewer (qualité générale)
- **Si fichiers de tests modifiés** : pr-test-analyzer
- **Si commentaires/docs ajoutés** : comment-analyzer
- **Si gestion des erreurs modifiée** : silent-failure-hunter
- **Si types ajoutés/modifiés** : type-design-analyzer
- **Après revue réussie** : code-simplifier (polish)

### 4. Lancer les agents de revue

Par défaut, lancer les agents **séquentiellement** (plus facile à suivre).
Si l'argument `parallel` est fourni, les lancer en parallèle.

Agents disponibles dans `agents/pr-review/` :
- `comment-analyzer` — Précision et maintenabilité des commentaires
- `pr-test-analyzer` — Couverture et qualité des tests
- `silent-failure-hunter` — Échecs silencieux et gestion des erreurs
- `type-design-analyzer` — Conception des types et invariants
- `code-reviewer` — Conformité aux conventions et bugs
- `code-simplifier` — Simplification et clarté

### 5. Consolider les résultats

```markdown
# Résumé de la revue PR

## Issues critiques (X trouvées)
- [agent]: Description [fichier:ligne]

## Issues importantes (X trouvées)
- [agent]: Description [fichier:ligne]

## Suggestions (X trouvées)
- [agent]: Suggestion [fichier:ligne]

## Points positifs
- Ce qui est bien fait dans cette PR

## Plan d'action recommandé
1. Corriger les issues critiques en premier
2. Traiter les issues importantes
3. Considérer les suggestions
4. Relancer la revue après les corrections
```

### 6. Plan d'action

Organiser les résultats par priorité et proposer un plan d'action clair.

## Exemples d'utilisation

```
/review-pr                    # Revue complète (défaut)
/review-pr tests errors       # Tests et gestion d'erreurs uniquement
/review-pr comments           # Commentaires uniquement
/review-pr simplify           # Simplification uniquement
/review-pr all parallel       # Tous les agents en parallèle
```

## Conseils

- **Lancer tôt** : avant de créer la PR, pas après
- **Focus sur les changements** : les agents analysent le git diff par défaut
- **Corriger le critique d'abord** : traiter les issues haute priorité avant le reste
- **Relancer après corrections** : vérifier que les issues sont résolues
