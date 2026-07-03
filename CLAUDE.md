# Anleitung für Agenten: Neues HA-Addon erstellen

Wenn ein neues Addon erstellt werden soll, immer nach diesem Schema vorgehen:

## 1. Verzeichnis anlegen
Slug in `snake_case`, z.B. `addon_beispiel/`. Slug muss mit `config.yaml: slug` übereinstimmen.

## 2. Dateien erzeugen (siehe `addon_example/` als Referenz)
- `config.yaml` — Pflichtfelder: name, version (SemVer, Start bei 0.1.0), slug, description, arch, init: false, startup, options + schema
- `build.yaml` — nur nötig, wenn ein eigenes Base-Image pro Architektur verwendet wird. Sonst weglassen und `image:` direkt in config.yaml setzen.
- `Dockerfile` — `ARG BUILD_FROM` + `FROM ${BUILD_FROM}`, danach Pakete/App
- `run.sh` — Shebang `#!/usr/bin/with-contenv bashio`, Optionen via `bashio::config`, ausführbar (`chmod +x`)
- `DOCS.md` — Installation + jede Option aus dem Schema erklären
- `CHANGELOG.md` — Start mit `## 0.1.0 - Initiales Release`
- `README.md` — kurz, verweist auf DOCS.md

## 3. Validierungsregeln (vom Agenten vor Commit selbst zu prüfen)
- `slug` in config.yaml == Verzeichnisname
- jede Option in `options:` hat einen passenden Eintrag in `schema:`
- `version` in config.yaml wird bei jeder inhaltlichen Änderung erhöht (SemVer)
- keine hartcodierten Secrets/Tokens im Dockerfile oder run.sh
- `ports:` nur setzen, wenn das Addon tatsächlich einen Port exposed
- bei Zugriff auf Home Assistant API: `homeassistant_api: true` statt `hassio_api: true`, wenn möglich (kleinere Berechtigung)

## 4. Nach dem Erstellen
- `README.md` im Repo-Root: neue Zeile in der Addon-Tabelle ergänzen
- Commit + PR öffnen, NICHT direkt auf `main` pushen — Lint-Workflow muss vorher grün sein
- PR-Beschreibung: kurze Zusammenfassung, was das Addon tut, welche Optionen es hat

## 5. Was der Agent NICHT automatisch tun soll
- Keine Secrets/API-Keys in Dateien committen, auch nicht in Beispielen (Platzhalter wie `<API_KEY>` verwenden)
- Kein direkter Push in Produktions-Branches ohne PR
- Bei Unsicherheit über sinnvolle `arch:`-Werte: alle 5 Standard-Architekturen aus `addon_example` übernehmen, nicht raten
