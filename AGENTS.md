# Anleitung für Agenten: Neues HA-Addon erstellen

Dieses Repo (`ha-addons`) ist das **zentrale Repository**, das Nutzer in Home Assistant hinzufügen. Es enthält nur schlanke `config.yaml`-Verweise auf vorgebaute GHCR-Images — kein Build-Code. Jedes Addon hat sein eigenes **Build-Repo** (`ha-addons-<slug>`, z.B. [ha-addons-grafana](https://github.com/bborchers/ha-addons-grafana)), das den tatsächlichen Dockerfile/build.yaml/run.sh-Code enthält, baut und nach GHCR pusht. Wiederverwendbare CI-Workflows liegen zentral in [ha-addons-workflow](https://github.com/bborchers/ha-addons-workflow).

Wenn ein neues Addon erstellt werden soll, immer nach diesem Schema vorgehen (Referenz: `ha-addons-grafana`).

## 1. Neues Build-Repo `ha-addons-<slug>` anlegen
Slug in `snake_case` oder kurzem Einwort-Format (z.B. `grafana`, `influxdb`). Slug muss überall identisch verwendet werden (Verzeichnisname, `config.yaml: slug`, GHCR-Image-Name, Repo-Suffix).

Repo-Struktur:
```
<slug>/
├── config.yaml     # Pflicht — version: "dev", KEIN image:-Feld
├── build.yaml      # Pflicht bei Multi-Arch Base-Images
├── Dockerfile       # Pflicht
├── run.sh           # Entrypoint
├── DOCS.md          # Pflicht — erscheint im HA UI
├── CHANGELOG.md      # empfohlen
└── icon.png / logo.png
.github/
├── workflows/lint.yml            # uses: bborchers/ha-addons-workflow/.github/workflows/lint.yml@main
├── workflows/commitlint.yml      # uses: bborchers/ha-addons-workflow/.github/workflows/commitlint.yml@main
├── workflows/release-drafter.yml # uses: .../release-drafter.yml@main
├── workflows/deploy.yml          # on: release published → uses: .../build-deploy.yml@main
└── release-drafter.yml           # Version-Resolver-Labels major/minor/patch
renovate.json           # config:recommended + dependencyDashboardApproval + semanticCommits:enabled
commitlint.config.cjs   # extends @commitlint/config-conventional
LICENSE                 # MIT, Bjoern Borchers
```

`config.yaml`-Pflichtfelder: name, version ("dev" im Build-Repo!), slug, description, arch, init: false, options + schema. **Kein `startup`/`boot`**, wenn der Default-Wert reicht — der Addon-Linter markiert redundante Defaults als Fehler.

`Dockerfile` — `ARG BUILD_FROM` + `FROM ${BUILD_FROM}`, danach Pakete/App. `run.sh` — Shebang `#!/usr/bin/with-contenv bashio`, Optionen via `bashio::config`, ausführbar (`chmod +x`).

Repo-Bootstrap (einmalig, per `gh` CLI):
- `gh repo create bborchers/ha-addons-<slug> --public`
- Erster Commit direkt auf `main` (noch ungeschützt), danach Branch Protection aktivieren (PR-Pflicht, Required Checks `commitlint / commitlint` + `lint / lint`, `enforce_admins: true`)
- `delete_branch_on_merge: true`, `allow_auto_merge: true` setzen
- Labels `major`/`minor`/`patch` anlegen (`gh label create`)
- Fine-grained PAT (Scope: nur `ha-addons`, Permissions `Contents: Read and write` + `Pull requests: Read and write`) vom Nutzer erfragen, als Secret `DISPATCH_TOKEN` in diesem neuen Repo hinterlegen
- Renovate GitHub App muss vom Nutzer manuell für das neue Repo freigeschaltet werden (kann der Agent nicht selbst)

## 2. Eintrag im zentralen Repo `ha-addons`
Neues Verzeichnis `<slug>/` mit:
- `config.yaml`: gleiche Felder wie im Build-Repo, aber `version` = aktueller echter SemVer-Wert (Start `0.1.0`) und zusätzliches Feld `image: "ghcr.io/bborchers/<slug>/{arch}"`. Kein Dockerfile/build.yaml/run.sh.
- `DOCS.md`, `CHANGELOG.md`, `icon.png`, `logo.png` (Kopie aus dem Build-Repo — wird ab dem ersten Release automatisch vom `repository-updater`-Workflow synchron gehalten)

`README.md` im Repo-Root: neue Zeile in der Addon-Tabelle ergänzen (Name, Beschreibung, Link zum Build-Repo).

## 3. Validierungsregeln (vom Agenten vor Commit selbst zu prüfen)
- `slug` identisch in beiden Repos (Verzeichnisname, config.yaml, GHCR-Image-Pfad)
- jede Option in `options:` hat einen passenden Eintrag in `schema:`
- keine hartcodierten Secrets/Tokens im Dockerfile oder run.sh
- `ports:` nur setzen, wenn das Addon tatsächlich einen Port exposed
- bei Zugriff auf Home Assistant API: `homeassistant_api: true` statt `hassio_api: true`, wenn möglich (kleinere Berechtigung)
- `arch:` nur Architekturen setzen, die die zugrunde liegende Anwendung tatsächlich unterstützt (nicht blind alle übernehmen) — **aktuell werden nur `aarch64` und `amd64` von `home-assistant/builder` überhaupt gebaut**, `armv7`/`armhf`/`i386` sind seit Home Assistant 2025.12 nicht mehr unterstützt
- Commit-Messages folgen Conventional Commits (`feat:`, `fix:`, `chore:`, …) — wird von `commitlint` in beiden Repos erzwungen

## 4. Nach dem Erstellen
- Commit + PR öffnen, NICHT direkt auf `main` pushen (Branch Protection erzwingt das in allen drei Repos, auch für Admins)
- PR-Beschreibung: kurze Zusammenfassung, was das Addon tut, welche Optionen es hat
- Im Build-Repo: PR mit Label `major`/`minor`/`patch` versehen, damit Release Drafter die richtige nächste Version berechnet

## 5. Was der Agent NICHT automatisch tun soll
- Keine Secrets/API-Keys in Dateien committen, auch nicht in Beispielen (Platzhalter wie `<API_KEY>` verwenden)
- Kein direkter Push in Produktions-Branches ohne PR
- Kein PAT selbst erzeugen (nicht möglich) — den Nutzer bitten, es zu erstellen und im Chat oder per `gh secret set` bereitzustellen
- Bei Unsicherheit über sinnvolle `arch:`-Werte: recherchieren statt raten (z.B. offizielle Docker-Image-Manifeste der Anwendung prüfen)
