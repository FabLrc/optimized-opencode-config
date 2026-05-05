# Guide des Outils MCP et Spéciaux

Référence centrale pour tous les agents. Consultez ce guide quand vous avez besoin d'un outil spécifique.

## Context7 — Documentation de Librairies Externes

- **Quand utiliser** : Vous rencontrez une librairie/framework externe inconnue et devez comprendre son API, ses patterns, ou ses bonnes pratiques
- **Ne pas utiliser pour** : Code interne du projet, librairies standard du langage, concepts génériques
- **Procédure obligatoire en 2 étapes** :

  **Étape 1 — Résoudre l'ID de la librairie**
  ```
  context7_resolve-library-id(
    libraryName: "Nom exact de la librairie (ex: 'React', 'Next.js', 'TanStack Query')",
    query: "Votre question sur ce que vous cherchez à faire"
  )
  ```
  Cet appel retourne une liste de librairies avec leur `libraryId` (format `/org/project`).

  **Étape 2 — Poser une question spécifique**
  ```
  context7_query-docs(
    libraryId: "L'ID exact retourné à l'étape 1 (ex: '/reactjs/react.dev')",
    query: "Votre question précise sur l'API, un pattern, ou un exemple de code"
  )
  ```

- **⚠️ Important** : `context7_query-docs` exige un `libraryId` valide obtenu via `context7_resolve-library-id`. Vous ne pouvez pas deviner l'ID.
- **Limite** : Ne pas utiliser plus de 3 fois par question. Si vous ne trouvez pas après 3 appels, utilisez le meilleur résultat.
- **Exemple** : Si vous voyez `import { useQuery } from '@tanstack/react-query'` et ne connaissez pas l'API, utilisez Context7

## WebFetch — Récupération de Contenu Web

- **Quand utiliser** : Besoin de documentation externe non disponible via Context7, spécifications, RFCs, articles techniques, pages de release notes
- **Ne pas utiliser pour** : Code interne, contenu accessible via Context7
- **Permission** : Nécessite confirmation utilisateur par design (effet de bord externe non prévisible)
- **Paramètres** :
  - `url` : URL complète et valide (HTTP automatiquement upgradé en HTTPS)
  - `format` : `"markdown"` (défaut), `"text"` ou `"html"`
  - `timeout` : optionnel, max 120 secondes
- **⚠️ Important** : N'utilisez WebFetch que si Context7 n'a pas fourni la réponse. Context7 est préférable car il fournit des exemples de code et une documentation structurée.

## Image Analysis — Analyse Visuelle

- **Quand utiliser** : PR incluant des screenshots UI, revue de design frontend, analyse de maquettes
- **Outils disponibles** :
  - `openrouter_image_analyze_image` — Analyse d'image générale
  - `openrouter_image_analyze_webpage_screenshot` — Analyse spécialisée de pages web
  - `openrouter_image_analyze_mobile_app_screenshot` — Analyse spécialisée d'apps mobiles
- **Paramètres** : `format: "json"` pour une analyse structurée, `focusArea` pour cibler un aspect

## Plannotator — Planification Interactive et Revue

- **Quand utiliser** : Validation interactive de plans d'architecture, revue de code interactive, archivage de décisions
- **Commandes disponibles** :
  - `/plannotator-review` — Ouvre l'interface de revue interactive
  - `/plannotator-annotate` — Annote un fichier markdown, HTML ou URL
  - `/plannotator-archive` — Parcourt les décisions de plan archivées
  - `/plannotator-last` — Annote le dernier message de l'assistant
- **Usage** : Utilisé automatiquement dans les commandes `feature-dev` et `review-pr` aux phases appropriées
