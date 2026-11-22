# Private DNS filtering

A separate Pi-hole v6 Compose stack provides DNS filtering for an authorized VPN client. It uses HaGeZi Multi NORMAL as its single DNS list. uBlock Origin continues to handle page-level filtering in supported browsers. A legitimate redirect on a blocked hostname can be restored with a narrow allow rule after reproducing the problem.

Pi-hole stays on loopback. A DNS forwarder on the same host binds to the VPN interface address. An interface firewall rule and per-device source restrictions limit access, and the password-protected admin UI uses private VPN HTTPS. The LAN and server workloads keep their usual DNS paths. MagicDNS continues to resolve private device names. The private homelab overview links to the admin page, and the service checker tracks the forwarder, the container's health, and the login endpoint.

The forwarder gives Pi-hole priority and uses a public resolver over authenticated HTTPS if Pi-hole fails. Filtering pauses during fallback and resumes after recovery. A whole-host or VPN outage needs a resolver on the client for automatic fallback, or a manual DNS switch on clients without one. Advertising a public resolver alongside a private one can let clients bypass filtering during normal operation.

[Tailscale DNS behavior](https://tailscale.com/docs/reference/dns-in-tailscale) · [CoreDNS forwarding](https://coredns.io/plugins/forward/)
