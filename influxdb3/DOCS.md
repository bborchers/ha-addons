# Home Assistant Add-on: InfluxDB 3 Core

This add-on provides [InfluxDB 3 Core](https://docs.influxdata.com/influxdb3/core/),
a time-series database for Home Assistant and other applications. The HTTP API
is available at `http://<home-assistant-ip>:8181`. InfluxDB 3 Explorer is
available at `http://<home-assistant-ip>:8080`.

## InfluxDB 3 Explorer

The add-on includes the official InfluxDB 3 Explorer. Its `explorer_mode`
defaults to `query`, which provides read-only Explorer functionality according
to the configured InfluxDB token permissions. Set it to `admin` to enable
database, token, and plugin administration.

## Configuration

```yaml
log_level: info
node_id: "home-assistant"
explorer_mode: query
without_auth: false
admin_token: ""
disable_telemetry: true
```

### Authentication

Authentication is enabled by default. Set `admin_token` before the first start
to provide a preconfigured admin token. If no token is configured, the add-on
creates an admin token automatically on first start and stores it in the
persistent add-on data directory. Existing tokens are reused on later starts.

The bundled Explorer is preconfigured automatically with a connection named
`Home Assistant InfluxDB 3` at `http://localhost:8181`, using the same admin
token. The database field is left empty so the connection does not assume a
database that the add-on has not created; select or create the desired database
in Explorer as needed.

For a trusted, isolated Home Assistant network, `without_auth: true` disables
authentication completely. Do not expose an unauthenticated instance to an
untrusted network.

### Option: `log_level`

Sets the InfluxDB log filter. Supported values are `error`, `warn`, `info`,
`debug`, and `trace`. Default: `info`.

### Option: `node_id`

Unique identifier for this server node. It is used as part of the local data
storage path and must contain only letters, numbers, and hyphens. Default:
`home-assistant`.

### Option: `admin_token`

Optional admin token used to initialize authentication on a new data directory.
If omitted, the add-on generates and persists an admin token automatically.
Keep this value secret. Existing generated tokens are reused on subsequent
starts.

When `without_auth: true` is enabled, no token is generated and Explorer is
preconfigured without a token. Do not expose an unauthenticated instance to an
untrusted network.

### Option: `disable_telemetry`

Disables InfluxDB telemetry uploads. Default: `true`.

## Data and backups

Database files and processing-engine plugins are stored below
`/data/influxdb3` and persist across add-on restarts and upgrades. Create a
Home Assistant backup before uninstalling the add-on; Supervisor removes an
add-on's private data directory during uninstall.

## Support

Please report issues in the [build repository](https://github.com/bborchers/ha-addons-influxdb3).
