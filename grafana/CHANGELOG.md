# Changelog

## 0.1.4

- Added missing changelog entry for 0.1.3 (no functional change)

## 0.1.3

- Added missing changelog entry for 0.1.2 (no functional change)

## 0.1.2

- Support link in DOCS.md now points to the build repo
- GHCR image renamed to `ha-addons-grafana/{arch}` (was `grafana/{arch}`)

## 0.1.1

- Removed `armv7` (no longer supported by Home Assistant since 2025.12, `home-assistant/builder` can no longer build this architecture)
- Removed `startup`/`boot` from `config.yaml` (were only default values, flagged as errors by the add-on linter)

## 0.1.0

- Initial release, based on Grafana OSS 13.1.0
