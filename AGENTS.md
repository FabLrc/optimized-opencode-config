## Outils MCP et Spéciaux à Disposition

Les agents doivent être conscients des outils à leur disposition et les utiliser judicieusement :

- **Context7** : Documentation et exemples de code pour librairies/frameworks externes. À utiliser quand une librairie inconnue est rencontrée.
- **WebFetch** : Récupération de documentation web externe. Nécessite confirmation utilisateur.
- **Image Analysis** : Analyse de screenshots UI/UX pour les revues frontend. Utilisé par l'agent dédié `ui-reviewer`.
- **Plannotator** : Revue interactive de plans et archivage de décisions. Invoqué automatiquement par `feature-dev` et `review-pr`.

Le fichier `docs/tool-guide.md` détaille la procédure de chaque outil pour l'agent principal. Les sous-agents (plan, build, feature-dev, etc.) ignorent ce fichier ; ils reçoivent les instructions d'outils directement via leur propre configuration.

# Coding Principles

When writing or modifying code, follow these principles. When reviewing code, verify adherence to them.

## SOLID

- **S**ingle Responsibility: A class or function does one thing and has one reason to change. Extract side responsibilities into dedicated units.
- **O**pen/Closed: Code is open for extension but closed for modification. Use interfaces, composition, or strategy patterns instead of modifying existing code.
- **L**iskov Substitution: Subtypes must be replaceable by their base types without breaking behavior. Never override a method with incompatible signatures or weaker invariants.
- **I**nterface Segregation: Prefer small, focused interfaces over large monolithic ones. Clients should not depend on methods they don't use.
- **D**ependency Inversion: Depend on abstractions, not concrete implementations. Inject dependencies; don't hard-code them.

## KISS — Keep It Simple, Stupid

- Prefer the simplest solution that satisfies all requirements.
- Do not build for hypothetical future needs. YAGNI (You Ain't Gonna Need It).
- Avoid premature abstractions. Duplication is cheaper than the wrong abstraction.
- Every layer of indirection must have a clear, justified purpose.

## Clean Code

- **Naming**: Use descriptive, pronounceable, searchable names. Avoid abbreviations unless universally understood.
- **Functions**: Keep functions small (ideally under 20 lines). A function should do one thing, do it well, and do it only.
- **Arguments**: Prefer 0-2 arguments. Use parameter objects for 3+ related arguments.
- **Comments**: Do not state the obvious. Comments explain WHY, not WHAT. Prefer self-documenting code.
- **Magic numbers**: Extract constants with meaningful names. No unexplained literals.
- **Error handling**: Use exceptions, not return codes. Prefer specific exception types. Never swallow errors silently.

## Boy Scout Rule

Always leave the code better than you found it. Every modification is an opportunity for micro-refactoring:

- Rename a poorly-named variable or function.
- Extract a small helper to reduce duplication.
- Add a missing type annotation or null check.
- Simplify a complex conditional.
- Remove dead or commented-out code.
- Fix a nearby typo or formatting inconsistency.

The improvement scope should be proportional to the original change — a one-line fix may justify a one-line cleanup, not a full refactor.
