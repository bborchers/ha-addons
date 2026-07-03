# Home Assistant Add-on Repository

![Project Stage](https://img.shields.io/badge/Project%20Stage-Active-brightgreen)
[![License](https://img.shields.io/github/license/bborchers/ha-addons)](LICENSE)
![Architectures](https://img.shields.io/badge/architectures-aarch64%20%7C%20amd64-blue)
[![Lint Addons](https://github.com/bborchers/ha-addons/actions/workflows/lint.yml/badge.svg)](https://github.com/bborchers/ha-addons/actions/workflows/lint.yml)
![Maintenance](https://img.shields.io/maintenance/yes/2026)
![Commits](https://img.shields.io/github/commit-activity/t/bborchers/ha-addons)

## Addons in diesem Repository

Dies ist das **zentrale Repository**, das in Home Assistant hinzugefügt wird. Es enthält pro Addon nur eine schlanke `config.yaml` mit Verweis auf ein vorgebautes Image auf GHCR — kein Build-Code. Der eigentliche Quellcode (Dockerfile, build.yaml, run.sh) liegt in einem eigenen Build-Repo pro Addon.

| Addon | Beschreibung | Version | Release-Datum | Build-Repo |
|---|---|---|---|---|
| [Grafana](grafana/) | Grafana Analytics- und Monitoring-Plattform | [![Version](https://img.shields.io/github/v/release/bborchers/ha-addons-grafana)](https://github.com/bborchers/ha-addons-grafana/releases/latest) | 2026-07-03 | [ha-addons-grafana](https://github.com/bborchers/ha-addons-grafana) |

## Installation als Repository

1. Home Assistant → Einstellungen → Add-ons → Add-on Store
2. Oben rechts (⋮) → Repositories
3. URL dieses Repos hinzufügen: `https://github.com/bborchers/ha-addons`

## Wie ein Addon-Update hierher kommt

1. Im Build-Repo (z.B. `ha-addons-grafana`) wird ein PR gemergt (mit Label `major`/`minor`/`patch`).
2. Release Drafter aktualisiert automatisch einen Draft-Release mit der nächsten Version.
3. Wird der Draft veröffentlicht, baut das Build-Repo das Multi-Arch-Image und pusht es nach `ghcr.io/bborchers/<slug>`.
4. Das Build-Repo löst per `repository_dispatch` einen Workflow hier aus (`.github/workflows/repository-updater.yml`), der `<slug>/config.yaml` automatisch auf die neue Version aktualisiert (per PR + Auto-Merge).

## Neues Addon hinzufügen (für Agenten/Automatisierung)

Ein neues Addon besteht aus drei Teilen:

1. **Neues Build-Repo** `ha-addons-<slug>` (analog zu `ha-addons-grafana`): enthält `<slug>/{config.yaml (version: dev), Dockerfile, build.yaml, run.sh, DOCS.md, CHANGELOG.md, icon.png, logo.png}`, plus `.github/workflows/{lint,commitlint,release-drafter,deploy}.yml` als dünne Wrapper um die reusable Workflows aus [ha-addons-workflow](https://github.com/bborchers/ha-addons-workflow), eigene `renovate.json`/`commitlint.config.cjs`/`LICENSE`, Labels `major`/`minor`/`patch`, Branch Protection (PR-Pflicht + Required Checks `commitlint / commitlint` + `lint / lint` + `enforce_admins`), Secret `DISPATCH_TOKEN` (gleiches PAT-Muster wie bei `ha-addons-grafana`, Scope nur auf dieses zentrale Repo).
2. **Eintrag hier** in `ha-addons`: neues Verzeichnis `<slug>/` mit schlanker `config.yaml` (`version` + `image: "ghcr.io/bborchers/<slug>/{arch}"`), `DOCS.md`, `CHANGELOG.md`, `icon.png`, `logo.png`. Diese Dateien werden ab dem ersten Release automatisch vom `repository-updater`-Workflow synchron gehalten.
3. **Tabellenzeile** in diesem README ergänzen (Addon, Beschreibung, Version-Badge `https://img.shields.io/github/v/release/bborchers/ha-addons-<slug>`, Release-Datum, Build-Repo-Link). Version und Release-Datum werden ab dann bei jedem `repository_dispatch` automatisch vom `repository-updater`-Workflow aktualisiert.

`.github/workflows/lint.yml` validiert jede `config.yaml` hier automatisch gegen das HA-Schema.
