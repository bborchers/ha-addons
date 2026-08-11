# Home Assistant Add-on Repository

![project stage](https://img.shields.io/badge/project%20stage-experimental-yellow)
[![license](https://img.shields.io/github/license/bborchers/ha-addons)](LICENSE)
![architectures](https://img.shields.io/badge/architectures-aarch64%20%7C%20amd64-blue)
[![lint addons](https://github.com/bborchers/ha-addons/actions/workflows/lint.yml/badge.svg)](https://github.com/bborchers/ha-addons/actions/workflows/lint.yml)
![maintenance](https://img.shields.io/maintenance/yes/2026)
![commits](https://img.shields.io/github/commit-activity/t/bborchers/ha-addons)

These add-ons are in an early, experimental stage — expect rough edges and breaking changes between releases.

## Add-ons in this repository

This is the **central repository** added to Home Assistant. For each add-on it contains only a lean `config.yaml` pointing to a pre-built image on GHCR — no build code. The actual source code (Dockerfile, build.yaml, run.sh) lives in a dedicated build repo per add-on.

| Add-on | Description | Version | Release Date | Build Repo |
|---|---|---|---|---|
| [InfluxDB 3 Core](influxdb3/) | InfluxDB 3 Core time-series database | [![Version](https://img.shields.io/github/v/release/bborchers/ha-addons-influxdb3)](https://github.com/bborchers/ha-addons-influxdb3/releases/latest) | 2026-08-09 | [ha-addons-influxdb3](https://github.com/bborchers/ha-addons-influxdb3) |
| [Grafana](grafana/) | Grafana analytics and monitoring platform | [![Version](https://img.shields.io/github/v/release/bborchers/ha-addons-grafana)](https://github.com/bborchers/ha-addons-grafana/releases/latest) | 2026-08-11 | [ha-addons-grafana](https://github.com/bborchers/ha-addons-grafana) |
| [Chrony](chrony/) | Chrony-based time synchronization service with configurable upstream servers and optional NTS | [![Version](https://img.shields.io/github/v/release/bborchers/ha-addons-chrony)](https://github.com/bborchers/ha-addons-chrony/releases/latest) | 2026-08-04 | [ha-addons-chrony](https://github.com/bborchers/ha-addons-chrony) |
| [Uptime Kuma](uptimekuma/) | Self-hosted uptime monitoring with a web interface | [![Version](https://img.shields.io/github/v/release/bborchers/ha-addons-uptimekuma)](https://github.com/bborchers/ha-addons-uptimekuma/releases/latest) | 2026-08-04 | [ha-addons-uptimekuma](https://github.com/bborchers/ha-addons-uptimekuma) |

## Installing as a repository

1. Home Assistant → Settings → Add-ons → Add-on Store
2. Top right (⋮) → Repositories
3. Add this repo's URL: `https://github.com/bborchers/ha-addons`

## How an add-on update gets here

1. A PR is merged in the build repo (e.g. `ha-addons-grafana`) with label `major`/`minor`/`patch`.
2. Release Drafter automatically updates a draft release with the next version.
3. When the draft is published, the build repo builds the multi-arch image and pushes it to `ghcr.io/bborchers/<slug>`.
4. The build repo triggers a workflow here via `repository_dispatch` (`.github/workflows/repository-updater.yml`), which automatically updates `<slug>/config.yaml` to the new version (via PR + auto-merge).

## Adding a new add-on (for agents/automation)

A new add-on consists of three parts:

1. **New build repo** `ha-addons-<slug>` (analogous to `ha-addons-grafana`): contains `<slug>/{config.yaml (version: dev), Dockerfile, build.yaml, run.sh, DOCS.md, CHANGELOG.md, icon.png, logo.png}`, plus `.github/workflows/{lint,commitlint,release-drafter,deploy}.yml` as thin wrappers around the reusable workflows from [ha-addons-workflow](https://github.com/bborchers/ha-addons-workflow), its own `renovate.json`/`commitlint.config.cjs`/`LICENSE`, labels `major`/`minor`/`patch`, branch protection (PR required + required checks `commitlint / commitlint` + `lint / lint` + `enforce_admins`), secret `DISPATCH_TOKEN` (same PAT pattern as `ha-addons-grafana`, scoped only to this central repo).
2. **An entry here** in `ha-addons`: a new `<slug>/` directory with a lean `config.yaml` (`version` + `image: "ghcr.io/bborchers/<slug>/{arch}"`), `DOCS.md`, `CHANGELOG.md`, `icon.png`, `logo.png`. From the first release onward, these files are kept in sync automatically by the `repository-updater` workflow.
3. **A table row** added to this README (add-on, description, version badge `https://img.shields.io/github/v/release/bborchers/ha-addons-<slug>`, release date, build repo link). From then on, the version and release date are automatically updated on every `repository_dispatch` by the `repository-updater` workflow.

`.github/workflows/lint.yml` automatically validates every `config.yaml` here against the HA schema.
