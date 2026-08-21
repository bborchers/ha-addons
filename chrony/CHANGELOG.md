# Changelog

## [0.7.8](https://github.com/bborchers/ha-addons-chrony/compare/v0.7.7...v0.7.8) (2026-08-21)

### Bug Fixes

* give chore(deps)/chore(ci) commits their own release notes section ([#27](https://github.com/bborchers/ha-addons-chrony/issues/27)) ([8399f7f](https://github.com/bborchers/ha-addons-chrony/commit/8399f7f08638915c318eb4ac7ff9da38b1cc4205))
* install a compatible conventional-changelog-conventionalcommits version ([#28](https://github.com/bborchers/ha-addons-chrony/issues/28)) ([1e604a5](https://github.com/bborchers/ha-addons-chrony/commit/1e604a57b76f31ba9690894097c3326258683103)), closes [#27](https://github.com/bborchers/ha-addons-chrony/issues/27) [47/#48](https://github.com/47/ha-addons-chrony/issues/48)

### CI Updates

* **ci:** disable Renovate automerge and assign PRs to bborchers ([#25](https://github.com/bborchers/ha-addons-chrony/issues/25)) ([a645bf9](https://github.com/bborchers/ha-addons-chrony/commit/a645bf9bc6fa60b8ad23af1ab85361f3b525f27c))
* **ci:** update github actions ([#24](https://github.com/bborchers/ha-addons-chrony/issues/24)) ([5064a68](https://github.com/bborchers/ha-addons-chrony/commit/5064a68c4fb68f18ba6f5f65d624946a8db69748))


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
