# Changelog

## [0.7.7](https://github.com/bborchers/ha-addons-chrony/compare/v0.7.6...v0.7.7) (2026-08-04)


### Bug Fixes

* run semantic-release directly via npx and label dependency bumps as fix(deps) ([#23](https://github.com/bborchers/ha-addons-chrony/issues/23)) ([3274ff5](https://github.com/bborchers/ha-addons-chrony/commit/3274ff5e6470dd83bacc4f6b53ef43bee92f9d8e))


## [0.7.6](https://github.com/bborchers/ha-addons-chrony/compare/v0.7.5...v0.7.6) (2026-08-04)


## 0.7.5

- No functional change (CI/Renovate automation only).

## 0.7.4

- Updated the hassio-addons base image to v21.0.1.

## 0.7.3

- No functional change (CI/Renovate automation only).

## 0.7.2

- No functional change (CI/Renovate automation only).

## 0.7.0

- Published UDP port 123 for NTP clients.
- Added configurable `allowed_networks` to restrict NTP client access by CIDR.

## 0.6.1

- Switched to the NTS-enabled Chrony package.

## 0.6.0

- Renamed the add-on and build repository to `chrony`.
- Corrected the upstream server-list schema for Home Assistant.

## 0.5.0

- Initial release after the repository history reset.

## 0.1.0

- Initial release of Chrony Time Server with configurable upstream time servers and optional NTS
