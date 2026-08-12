# Home Assistant Add-on: VictoriaMetrics

This add-on provides the open-source [VictoriaMetrics single-node server](https://docs.victoriametrics.com/victoriametrics/), a fast and resource-efficient time-series database for Prometheus-compatible metrics.

The HTTP API and built-in web UI are available at `http://<home-assistant-ip>:8428`.

## Configuration

```yaml
log_level: INFO
retention_period: "1"
```

### Option: `log_level`

Controls VictoriaMetrics log verbosity. Supported values are `ERROR`, `WARN`, `INFO`, and `DEBUG`. Default: `INFO`.

### Option: `retention_period`

Retention period in months. VictoriaMetrics removes samples older than this period. The default is `1` month. See the [VictoriaMetrics retention documentation](https://docs.victoriametrics.com/victoriametrics/#retention).

## Data and backups

All time-series data is stored below `/data/victoriametrics` in the persistent add-on data directory. Create a Home Assistant backup before uninstalling the add-on; Supervisor removes an add-on's private data directory during uninstall.

## Support

Please report issues in the [build repository](https://github.com/bborchers/ha-addons-victoriametrics).
