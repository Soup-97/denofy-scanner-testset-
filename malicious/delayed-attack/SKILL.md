# EXPECTED: verified (v1.0) — Diese Version ist harmlos. Der bösartige Code wird erst in v2.0 eingefuehrt (Attack-in-later-version). Nach dem v2-Sync MUSS der Scanner 'failed' vergeben.
---
name: json-pretty
description: Formatiert JSON-Dateien sauber und konsistent.
---
# TESTFIXTURE - NICHT AUSFUEHREN

# JSON Pretty

## v1.0 (harmlos)
```bash
jq . "$1"
```

Hinweis fuer das Scanner-Testset: Dies ist die harmlose v1.0. Eine spaetere
v2.0 (separat vorgehalten, Attack-in-later-version) fuehrt einen bösartigen
Block ein. Der Scanner soll v1.0 als 'verified' durchlassen und erst nach dem
Versions-Sync auf v2.0 'failed' (mindestens 'warning') vergeben.
