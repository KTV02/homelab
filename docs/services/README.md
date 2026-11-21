# Services

| Service | Type | Runtime | Page |
|---|---|---|---|
| Jellyfin | Off-the-shelf | Native systemd service | [jellyfin.md](jellyfin.md) |
| OpenCloud | Off-the-shelf, custom deployment | Native systemd services | [opencloud.md](opencloud.md) |
| PdfSearch | Self-developed | Docker Compose | [pdfsearch.md](pdfsearch.md) |
| Mail archive | Self-developed | Docker Compose | [mail-archive.md](mail-archive.md) |
| Forest hut IoT | Self-developed | Native services on both Pis | [forest-hut.md](forest-hut.md) |
| Job coordinator | Self-developed on Node-RED | Native systemd service | [job-coordinator.md](job-coordinator.md) |
| Monitoring | Off-the-shelf + custom | Native + timers | [../monitoring.md](../monitoring.md) |

## Common baseline

Every service follows the same baseline: loopback binding, access through the VPN mesh TLS proxy, its own authentication, hardened container settings, monitoring, and a deliberate backup decision.
