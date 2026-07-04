# Changelog

## 0.2.2

- Added missing changelog entry for 0.2.1 (no functional change)

## 0.2.1

- Added missing changelog entries for 0.1.5 and 0.2.0 (no functional change)

## 0.2.0

- Added optional SMTP settings (`smtp_enabled`, `smtp_host`, `smtp_user`, `smtp_password`, `smtp_from_address`, `smtp_from_name`, `smtp_skip_verify`, `smtp_starttls_policy`)
- Added optional `root_url` and `domain` options (`GF_SERVER_ROOT_URL` / `GF_SERVER_DOMAIN`)
- Added optional `ssl`, `certfile`, `keyfile` options to enable HTTPS using certificates from Home Assistant's `/ssl` directory
- Documented manual migration from the hassio-addons Grafana add-on

## 0.1.5

- Added missing changelog entry for 0.1.4 (no functional change)

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
