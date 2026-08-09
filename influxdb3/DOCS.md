# Home Assistant Add-on: InfluxDB 3 Core

This add-on provides [InfluxDB 3 Core](https://docs.influxdata.com/influxdb3/core/),
a time-series database for Home Assistant and other applications. The HTTP API
is available at `http://<home-assistant-ip>:8181`.

## Configuration

```yaml
log_level: info
node_id: "home-assistant"
without_auth: false
admin_token: ""
disable_telemetry: true
```

### Authentication

Authentication is enabled by default. Set `admin_token` before the first start
to provide a preconfigured admin token. The token is stored in the persistent
add-on data directory and is only used to initialize a new database.

If no token is configured, create the initial operator token from the add-on
terminal after starting the add-on:

```sh
influxdb3 create token --admin
```

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
Keep this value secret. Existing tokens are not replaced on subsequent starts.

### Option: `disable_telemetry`

Disables InfluxDB telemetry uploads. Default: `true`.

## Data and backups

Database files and processing-engine plugins are stored below
`/data/influxdb3` and persist across add-on restarts and upgrades. Create a
Home Assistant backup before uninstalling the add-on; Supervisor removes an
add-on's private data directory during uninstall.

## Support

Please report issues in the [build repository](https://github.com/bborchers/ha-addons-influxdb3).
