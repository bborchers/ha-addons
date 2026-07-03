# Home Assistant Add-on: Grafana

This add-on provides [Grafana](https://grafana.com/) (an analytics and monitoring platform). It's useful, for example, to visualize data from InfluxDB, Prometheus, or other data sources on your local network.

## Installation

1. Add the repository under Settings → Add-ons → Add-on Store → Repositories.
2. Find the "Grafana" add-on in the list and install it.
3. Adjust the configuration (see below).
4. Start the add-on.
5. Grafana is then reachable at `http://<home-assistant-ip>:3000`.

## Configuration

Example configuration:

```yaml
log_level: info
admin_user: "admin"
admin_password: "a-secure-password"
anonymous_access: false
```

### Option: `log_level`

Controls the verbosity of the Grafana logs. Possible values: `debug`, `info`, `warn`, `error`, `critical`. Default: `info`.

### Option: `admin_user`

Username for the initial Grafana administrator account. Default: `admin`.

### Option: `admin_password`

Password for the initial Grafana administrator account. **Should be changed after first login and set to a secure password in the add-on configuration.** Default: `admin`.

### Option: `anonymous_access`

If enabled (`true`), users can access Grafana with read permissions without logging in. Default: `false`.

## Data

The Grafana database, logs, and plugins are stored in the persistent add-on data directory (`/data/grafana`) and are preserved across restarts and updates.

## Support

If you run into problems, please open an issue in the [build repository](https://github.com/bborchers/ha-addons-grafana).
