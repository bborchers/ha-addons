# Home Assistant Add-on: Grafana

Dieses Addon stellt [Grafana](https://grafana.com/) (Analytics- und Monitoring-Plattform) bereit. Es eignet sich z.B. zur Visualisierung von Daten aus InfluxDB, Prometheus oder anderen Datenquellen im lokalen Netzwerk.

## Installation

1. Repository unter Einstellungen → Add-ons → Add-on Store → Repositories hinzufügen.
2. Addon "Grafana" in der Liste suchen und installieren.
3. Konfiguration anpassen (siehe unten).
4. Addon starten.
5. Grafana ist danach über `http://<home-assistant-ip>:3000` erreichbar.

## Konfiguration

Beispielkonfiguration:

```yaml
log_level: info
admin_user: "admin"
admin_password: "ein-sicheres-passwort"
anonymous_access: false
```

### Option: `log_level`

Steuert die Ausführlichkeit der Grafana-Logs. Mögliche Werte: `debug`, `info`, `warn`, `error`, `critical`. Standard: `info`.

### Option: `admin_user`

Benutzername des initialen Grafana-Administrator-Kontos. Standard: `admin`.

### Option: `admin_password`

Passwort des initialen Grafana-Administrator-Kontos. **Sollte nach der ersten Anmeldung geändert und in der Addon-Konfiguration auf ein sicheres Passwort angepasst werden.** Standard: `admin`.

### Option: `anonymous_access`

Wenn aktiviert (`true`), können Nutzer ohne Anmeldung mit Leserechten auf Grafana zugreifen. Standard: `false`.

## Daten

Grafana-Datenbank, Logs und Plugins werden im persistenten Addon-Datenverzeichnis (`/data/grafana`) gespeichert und bleiben bei Neustarts und Updates erhalten.

## Support

Bei Problemen bitte ein Issue im [Build-Repository](https://github.com/bborchers/ha-addons-grafana) öffnen.
