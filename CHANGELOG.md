# Changelog

Notable changes to the homelab (infrastructure, services, practices). Newest first.

## 2026-11
- Added OpenCloud as personal cloud storage on a dedicated HDD, replacing the previous file server (identity-aware ingress, OpenID Connect, playback-aware CPU governor)
- Controlled container image updater with dry-run mode
- Storage: fixed free-space accounting of the fast tier and cleaned up unused data

## 2026-10
- Mail archive: gated provider deletion based on signed restore proofs
- Docker-aware firewall filter for IPv4 and IPv6, tested in practice

## Foundation
- Raspberry Pi 5 main server on NVMe, tiered storage with mergerfs
- Jellyfin, PdfSearch, forest hut dashboard and hut node
- Node-RED job coordinator, including a security hardening pass (authenticated admin, per-client tokens, restricted container access)
- restic online and offline backups, Netdata monitoring
