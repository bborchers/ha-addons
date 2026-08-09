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

Authentication is enabled by default. Set `admin_token` before the first start
to provide a preconfigured admin token. If no token is configured, create the
initial operator token from the add-on terminal after starting it:

```sh
influxdb3 create token --admin
```

`without_auth: true` disables authentication completely and should only be used
on a trusted, isolated network. Data and plugins persist below `/data/influxdb3`.

See the [build repository](https://github.com/bborchers/ha-addons-influxdb3) for
support and the full documentation.
