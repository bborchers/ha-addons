# Uptime Kuma

Uptime Kuma is a self-hosted service monitor comparable to UptimeRobot. It can
monitor HTTP(S), TCP, DNS, and other endpoints, and send downtime notifications.

## Installation

1. Add `https://github.com/bborchers/ha-addons` as an Add-on Store repository.
2. Install **Uptime Kuma** and start it.
3. Open the web interface on port 3001 and complete the first-run setup.

## Configuration

Uptime Kuma is configured entirely in its web interface. Its data, monitor
definitions, and history are stored in `/data`, which persists across restarts
and updates.

## Home Assistant discovery

The add-on advertises the Uptime Kuma integration to Home Assistant. If it is
not discovered automatically, add the integration from **Settings → Devices &
Services**.

## Support

Please report issues in the [build repository](https://github.com/bborchers/ha-addons-uptimekuma/issues).
