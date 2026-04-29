---
description: Supprime les branches git locales dont le remote a été supprimé (marquées [gone])
subtask: true
---

## Ta tâche

Nettoyer les branches locales obsolètes dont le remote a été supprimé.

1. **Lister les branches pour identifier celles avec le statut [gone]**
   ```bash
   git branch -v
   ```
   Note : les branches avec le préfixe '+' ont des worktrees associés à supprimer d'abord.

2. **Identifier les worktrees à supprimer pour les branches [gone]**
   ```bash
   git worktree list
   ```

3. **Supprimer les worktrees et les branches [gone]**
   ```bash
   git branch -v | grep '\[gone\]' | sed 's/^[+* ]//' | awk '{print $1}' | while read branch; do
     echo "Traitement de la branche : $branch"
     worktree=$(git worktree list | grep "\\[$branch\\]" | awk '{print $1}')
     if [ ! -z "$worktree" ] && [ "$worktree" != "$(git rev-parse --show-toplevel)" ]; then
       echo "  Suppression du worktree : $worktree"
       git worktree remove --force "$worktree"
     fi
     echo "  Suppression de la branche : $branch"
     git branch -D "$branch"
   done
   ```

Si aucune branche n'est marquée [gone], rapporte qu'aucun nettoyage n'était nécessaire.
