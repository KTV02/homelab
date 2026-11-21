# Jellyfin

> Personal media server for my own film, series, music and audiobook collection, streamed to TV, phone and laptop.

| | |
|---|---|
| Type | Off-the-shelf ([jellyfin.org](https://jellyfin.org/)) |
| Runtime | Native systemd service with the Jellyfin-specific FFmpeg build |
| Status | In production |

## Why native instead of Docker?

Running Jellyfin directly on the host keeps media access and transcoding simple and avoids an extra layer for the most performance-critical service on a small ARM board.

## Access

- Inside the VPN mesh via the TLS proxy, like every other service
- On the home LAN as a deliberate exception, so TVs and media players without a VPN client can use it

## Storage

The library is read from the unified [mergerfs view](../storage.md): recent media on NVMe, older media on the HDD. For Jellyfin this is one directory tree.

## Playback has priority

Jellyfin is the service where performance is noticed immediately. The [job coordinator](job-coordinator.md) reads the current playback state through the Jellyfin API:

- While something is playing, no new heavy jobs (indexing, file moves, SMART long tests) are started.
- Cooperative jobs that are already running get a pause signal.
- Backups and backup maintenance have their own playback guard.
- Jellyfin itself is never stopped, restarted or throttled by automation.
