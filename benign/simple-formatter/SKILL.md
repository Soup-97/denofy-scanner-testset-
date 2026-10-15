# EXPECTED: verified — Harmloser Code-Formatierer, keine Netzwerk-/Dateizugriffe ausserhalb des Projekts
---
name: simple-formatter
description: Formatiert Quellcode mit gaengigen Formatierern (prettier, black, gofmt).
---
# Simple Formatter

Dieser Skill formatiert Dateien im aktuellen Projekt.

```bash
prettier --write "src/**/*.{js,ts}"
black .
gofmt -w .
```
