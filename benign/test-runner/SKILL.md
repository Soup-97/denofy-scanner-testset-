# EXPECTED: verified — Fuehrt lokale Tests aus, keine externen Downloads/Exfiltration
---
name: test-runner
description: Erkennt das Testframework und fuehrt die Testsuite lokal aus.
---
# Test Runner

```bash
if [ -f package.json ]; then npm test; fi
if [ -f pytest.ini ] || [ -f pyproject.toml ]; then pytest; fi
if [ -f go.mod ]; then go test ./...; fi
```
