# Services

| Service | Type | Runtime | Page |
|---|---|---|---|
| Jellyfin | Off-the-shelf | Native systemd service | [jellyfin.md](jellyfin.md) |
| OpenCloud | Off-the-shelf, custom deployment | Native systemd services | [opencloud.md](opencloud.md) |
| PdfSearch | Self-developed | Docker Compose | [pdfsearch.md](pdfsearch.md) |
| Mail archive | Self-developed | Docker Compose | [mail-archive.md](mail-archive.md) |
| Forest hut IoT | Self-developed | Native services on both Pis | [forest-hut.md](forest-hut.md) |
| Job coordinator | Self-developed on Node-RED | Native systemd service | [job-coordinator.md](job-coordinator.md) |
| Pi-hole | Off-the-shelf, custom deployment | Docker Compose with a native CoreDNS forwarder | [Private DNS filtering](../dns-filtering.md) |
| Monitoring | Off-the-shelf + custom | Native + timers | [../monitoring.md](../monitoring.md) |

## Common baseline

Web interfaces bind to loopback and use the private VPN TLS proxy with application-level authentication. The DNS forwarder binds to the VPN interface and restricts access through the VPN policy. Services have health checks, and container settings and backups are configured according to each service's needs.
