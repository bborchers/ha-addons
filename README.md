# Home Assistant Add-on Repository

[![Release](https://img.shields.io/github/v/release/bborchers/ha-addons)](https://github.com/bborchers/ha-addons/releases/latest)
![Project Stage](https://img.shields.io/badge/Project%20Stage-Active-brightgreen)
[![License](https://img.shields.io/github/license/bborchers/ha-addons)](LICENSE)
![Architectures](https://img.shields.io/badge/architectures-aarch64%20%7C%20amd64-blue)
[![Lint Addons](https://github.com/bborchers/ha-addons/actions/workflows/lint.yml/badge.svg)](https://github.com/bborchers/ha-addons/actions/workflows/lint.yml)
[![Build Addons](https://github.com/bborchers/ha-addons/actions/workflows/build.yml/badge.svg)](https://github.com/bborchers/ha-addons/actions/workflows/build.yml)
![Maintenance](https://img.shields.io/maintenance/yes/2026)
![Commits](https://img.shields.io/github/commit-activity/t/bborchers/ha-addons)

## Addons in diesem Repository

| Addon | Beschreibung |
|---|---|
| [Grafana](grafana/) | Grafana Analytics- und Monitoring-Plattform |

## Installation als Repository

1. Home Assistant → Einstellungen → Add-ons → Add-on Store
2. Oben rechts (⋮) → Repositories
3. URL dieses Repos hinzufügen: `https://github.com/bborchers/ha-addons`

## Neues Addon hinzufügen (für Agenten/Automatisierung)

Jedes Addon liegt in einem eigenen Verzeichnis mit mindestens:

```
<addon-slug>/
├── config.yaml     # Pflicht - Metadaten & Options-Schema
├── build.yaml      # Pflicht bei Multi-Arch Base-Images
├── Dockerfile       # Pflicht
├── run.sh           # Entrypoint
├── DOCS.md          # Pflicht - erscheint im HA UI
├── CHANGELOG.md      # empfohlen
└── README.md
```

Nach dem Hinzufügen eines neuen Addon-Verzeichnisses:
1. `.github/workflows/lint.yml` validiert config.yaml automatisch gegen das HA-Schema (via `frenck/action-addon-lint`).
2. `.github/workflows/build.yml` baut bei Push auf `main` alle geänderten Addons multi-arch und pusht sie nach GHCR.

Ein Agent, der neue Addons generiert, sollte sich strikt an diese Struktur halten, damit beide Workflows ohne Anpassung greifen.
