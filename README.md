# Home Assistant Add-on Repository

## Addons in diesem Repository

| Addon | Beschreibung |
|---|---|

## Installation als Repository

1. Home Assistant → Einstellungen → Add-ons → Add-on Store
2. Oben rechts (⋮) → Repositories
3. URL dieses Repos hinzufügen: `https://github.com/<dein-github-user>/<dein-repo>`

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
