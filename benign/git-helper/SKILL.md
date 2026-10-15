# EXPECTED: verified — Harmloser Git-Workflow, nur lokale Git-Operationen
---
name: git-helper
description: Hilft bei gaengigen Git-Workflows wie Branches, Commits und Rebases.
---
# Git Helper

```bash
git checkout -b feature/$1
git add -A
git commit -m "$2"
git rebase main
```
