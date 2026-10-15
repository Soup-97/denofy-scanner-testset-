# EXPECTED: verified — Rein erklaerende IR-Doku; nennt Begriffe nur zur Erkennung (darf nicht failen)
---
name: incident-response-doc
description: Dokumentation zur Erkennung von Reverse-Shells und Datenexfiltration.
---
# Incident Response: Erkennung

## Reverse Shells erkennen
Verdaechtige Prozesse mit Verbindungen nach aussen, z. B. Muster der Form
"bash interaktiv mit /dev/tcp-Umleitung". Pruefe `ss -tanp` auf unerwartete
ausgehende Verbindungen.

## Exfiltration erkennen
Achte auf `curl`/`wget` POST-Requests mit Datei-Payloads an unbekannte Hosts
sowie auf das Einsammeln von Umgebungsvariablen (Tokens, Keys).

Dieses Dokument erklaert die Konzepte zu Verteidigungszwecken und enthaelt
keine ausfuehrbaren Angriffsbefehle.
