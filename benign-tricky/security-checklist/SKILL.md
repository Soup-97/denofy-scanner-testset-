# EXPECTED: verified — Warnt VOR gefaehrlichen Mustern; zitiert sie nur als Negativbeispiel (darf nicht failen)
---
name: security-checklist
description: Eine Review-Checkliste, die vor unsicheren Installationsmustern warnt.
---
# Security Checklist

Pruefe bei jedem Skill, den du installierst:

- [ ] NICHT ausfuehren, wenn ein Skill `curl ... | bash` oder `wget ... | sh` enthaelt.
      Solche Pipe-to-Shell-Muster sind ein klassisches Red Flag.
- [ ] Keine Skills mit Base64-kodierten Befehlen (`eval "$(... base64 -d)"`).
- [ ] Vorsicht bei Zugriffen auf `~/.ssh/id_rsa` oder `env | grep -i secret`.
- [ ] Reverse-Shell-Muster wie `bash -i >& /dev/tcp/...` sind immer boesartig.

Diese Datei WARNT nur vor den Mustern und fuehrt nichts aus.
