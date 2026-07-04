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

### Option: `root_url`

Sets `GF_SERVER_ROOT_URL`. The full URL used to access Grafana from a browser, e.g. `https://grafana.example.com/` or `https://homeassistant.local:3000/`. Useful when Grafana is reachable through a reverse proxy or a different hostname than the default. Optional, not set by default.

### Option: `domain`

Sets `GF_SERVER_DOMAIN`. The domain name Grafana is served from, e.g. `grafana.example.com`. Optional, not set by default.

### SSL options

Enables HTTPS using the certificate/key pair from Home Assistant's `/ssl` directory (the same directory managed under Settings → Add-ons → your certificate add-on, e.g. Let's Encrypt).

```yaml
ssl: true
certfile: fullchain.pem
keyfile: privkey.pem
```

#### Option: `ssl`

Enables HTTPS. When `true`, sets `GF_SERVER_PROTOCOL=https`, `GF_SERVER_CERT_FILE=/ssl/<certfile>`, and `GF_SERVER_CERT_KEY=/ssl/<keyfile>`. Default: `false`.

#### Option: `certfile`

Filename of the certificate file inside Home Assistant's `/ssl` directory. Default: `fullchain.pem`.

#### Option: `keyfile`

Filename of the private key file inside Home Assistant's `/ssl` directory. Default: `privkey.pem`.

### SMTP options

These options configure Grafana's built-in email sender (used for alert notifications, invites, and password resets). All fields except `smtp_enabled` are optional; only set the ones you need.

```yaml
smtp_enabled: true
smtp_host: "smtp.example.com:587"
smtp_user: "smtp-user"
smtp_password: "a-secure-password"
smtp_from_address: "grafana@example.com"
smtp_from_name: "Grafana"
smtp_skip_verify: false
smtp_starttls_policy: "MandatoryStartTLS"
```

#### Option: `smtp_enabled`

Sets `GF_SMTP_ENABLED`. Enables Grafana's SMTP email sender. Default: `false`.

#### Option: `smtp_host`

Sets `GF_SMTP_HOST`. The SMTP server address, including port, e.g. `smtp.example.com:587`.

#### Option: `smtp_user`

Sets `GF_SMTP_USER`. Username for SMTP authentication.

#### Option: `smtp_password`

Sets `GF_SMTP_PASSWORD`. Password for SMTP authentication.

#### Option: `smtp_from_address`

Sets `GF_SMTP_FROM_ADDRESS`. The email address used as the sender for outgoing emails.

#### Option: `smtp_from_name`

Sets `GF_SMTP_FROM_NAME`. The display name used as the sender for outgoing emails.

#### Option: `smtp_skip_verify`

Sets `GF_SMTP_SKIP_VERIFY`. If `true`, skips verification of the SMTP server's certificate chain and hostname. Only use this for trusted internal servers with self-signed certificates.

#### Option: `smtp_starttls_policy`

Sets `GF_SMTP_STARTTLS_POLICY`. Controls StartTLS behavior. Possible values: `OpportunisticStartTLS`, `MandatoryStartTLS`, `NoStartTLS`.

## Data

The Grafana database, logs, and plugins are stored in the persistent add-on data directory (`/data/grafana`) and are preserved across restarts and updates.

## Migrating from the hassio-addons Grafana add-on

If you previously used the community [hassio-addons Grafana add-on](https://github.com/hassio-addons/addon-grafana), its data lives in that add-on's own private data directory (typically `/mnt/data/supervisor/addons/data/a0d7b954_grafana` on the host) and is **not** accessible to this add-on automatically — Home Assistant Supervisor add-ons are sandboxed from each other's private data by design, and this add-on cannot read it during startup.

To migrate manually, before starting this add-on for the first time:

1. Make sure the old hassio-addons Grafana add-on is stopped.
2. Using the [Terminal & SSH add-on](https://github.com/hassio-addons/addon-ssh) (or another add-on with host filesystem access), copy the old data directory into this add-on's data directory:
   ```sh
   cp -a /mnt/data/supervisor/addons/data/a0d7b954_grafana/. /mnt/data/supervisor/addons/data/f7a88481_grafana/grafana/
   ```
   `f7a88481_grafana` is this add-on's data directory: Home Assistant derives it from the [ha-addons](https://github.com/bborchers/ha-addons) repository URL (first 8 hex characters of its SHA1 hash) followed by the add-on slug `grafana`. You can confirm it exists under `/mnt/data/supervisor/addons/data/`.
3. Start this add-on. Grafana will pick up the existing database, dashboards, and plugins from `/data/grafana`.
4. Once you've confirmed the migration worked, you can uninstall the old add-on.

## Support

If you run into problems, please open an issue in the [build repository](https://github.com/bborchers/ha-addons-grafana).
