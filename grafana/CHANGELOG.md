# Changelog

## 0.1.3

- Changelog für 0.1.2 nachgetragen (keine funktionale Änderung)

## 0.1.2

- Support-Link in DOCS.md zeigt jetzt aufs Build-Repo
- GHCR-Image umbenannt zu `ha-addons-grafana/{arch}` (statt `grafana/{arch}`)

## 0.1.1

- `armv7` entfernt (von Home Assistant seit 2025.12 nicht mehr unterstützt, `home-assistant/builder` kann diese Architektur nicht mehr bauen)
- `startup`/`boot` aus `config.yaml` entfernt (waren nur Default-Werte, wurden vom Addon-Linter als Fehler markiert)

## 0.1.0

- Initiales Release, basierend auf Grafana OSS 13.1.0
