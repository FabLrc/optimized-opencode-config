---
description: Revue de code automatisée avec agents spécialisés (bugs, qualité, conventions)
argument-hint: "[--comment] [PR_NUMBER ou branche]"
---

# Revue de Code Automatisée

Effectue une revue de code approfondie en lançant plusieurs agents spécialisés en parallèle avec un système de scoring par confiance pour filtrer les faux positifs.

**Arguments optionnels :** "$ARGUMENTS"

## Contexte Git

- Diff actuel : !`git diff HEAD`
- Fichiers modifiés : !`git diff --name-only HEAD`
- Branche courante : !`git branch --show-current`
- Derniers commits : !`git log --oneline -5`

## Workflow de revue

### Étape 1 — Vérification préliminaire

Lance un agent haiku pour vérifier :
- Les changements sont-ils triviaux ou vides ? (si oui, stop)
- Y a-t-il un fichier CLAUDE.md ou opencode.md ou AGENTS.md à la racine ou dans les répertoires concernés ? (noter les chemins)

### Étape 2 — Analyse des conventions

Lance un agent pour lire et résumer les règles du projet (depuis CLAUDE.md / opencode.md si trouvé) et retourner une liste des règles clés.

### Étape 3 — Revue parallèle par 4 agents spécialisés

Lance ces 4 agents **en parallèle**, en leur transmettant le diff et les règles du projet :

**Agent A — Conformité aux conventions (sonnet)**
Vérifie le respect strict des règles du fichier CLAUDE.md/opencode.md. Ne retourne que les violations explicites et prouvables. Score de confiance 0-100 par issue.

**Agent B — Conformité aux conventions bis (sonnet)**
Même mission qu'Agent A, en parallèle pour double couverture. Compare les résultats.

**Agent C — Détection de bugs (opus)**
Analyse uniquement le diff. Cherche : erreurs de logique évidentes, null/undefined non gérés, race conditions, problèmes de sécurité, imports manquants. Ne flag que les bugs certains (pas d'hypothèses sur le contexte hors-diff). Score de confiance 0-100.

**Agent D — Problèmes d'implémentation (opus)**
Cherche dans le code introduit : sécurité (injection, XSS, etc.), logique incorrecte, typos dans les strings ou conditions, problèmes de performance évidents. Score 0-100.

### Étape 4 — Validation par agents

Pour chaque issue remontée par les agents C et D avec confiance ≥ 70, lance un agent de validation dédié pour confirmer que le problème est réel (pas un faux positif). Utilise sonnet pour la conformité, opus pour les bugs.

### Étape 5 — Filtrage et consolidation

**Ne garder que les issues validées avec confiance ≥ 80.**

Filtrer explicitement :
- Les problèmes pré-existants (non introduits par ce diff)
- Les nitpicks styliques non mentionnés dans les conventions
- Les issues hypothétiques dépendant d'un contexte extérieur au diff
- Ce qu'un linter attraperait automatiquement

### Étape 6 — Rapport final

```markdown
## Résultat de la revue

### Issues critiques (X trouvées)
- [agent]: Description — `fichier:ligne`

### Issues importantes (X trouvées)  
- [agent]: Description — `fichier:ligne`

### Aucune issue trouvée
"Revue terminée. Aucun bug ni violation de conventions détectés."
```

Si l'argument `--comment` est fourni et que des issues existent, créer un fichier `review-output.md` dans le répertoire courant avec le rapport complet.

## Notes

- Ne modifier aucun fichier sauf si explicitement demandé
- Créer une todo list avant de commencer
- Citer les règles exactes de CLAUDE.md lors des violations de conventions
