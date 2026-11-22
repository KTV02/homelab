# Pi-hole over Tailscale

I run Pi-hole v6 in Docker Compose on the home Raspberry Pi. It uses HaGeZi Multi NORMAL to block requests for known ad and tracking domains from authorized Tailscale devices.

## How a DNS request flows

1. A device sends a DNS query over Tailscale to CoreDNS, a forwarder on the Pi.
2. CoreDNS passes the query to Pi-hole on the same machine.
3. For a listed domain, Pi-hole replies with an address the device cannot use to reach the ad or tracker. For other domains, Pi-hole looks up the address through its upstream DNS resolver.

MagicDNS continues to resolve Tailscale device names. Devices on the home LAN have their own DNS settings.

## What happens during an outage

| Situation | What the device can expect |
| --- | --- |
| Pi-hole stops responding while the Pi is online | CoreDNS tries Google Public DNS over HTTPS. Queries sent to Google during the outage bypass filtering. CoreDNS resumes using Pi-hole when it recovers. |
| The Pi or the route to it is unavailable | Tailscale also provides Google as a separate resolver. A device can still resolve ordinary domains if it can reach Google directly. |

Tailscale can query both advertised resolvers at the same time. Google can answer first even while Pi-hole is healthy, so this setup favors DNS availability and filtering is best-effort. [Tailscale resolver ordering](https://tailscale.com/docs/reference/dns-in-tailscale#the-order-of-dns-resolvers) · [CoreDNS forwarding](https://coredns.io/plugins/forward/)

## Access and monitoring

CoreDNS accepts queries on the Tailscale interface. The firewall and Tailscale access policy restrict who can reach it. Pi-hole stays on loopback, and its password-protected admin page is available over private HTTPS from the homelab overview. The service checker monitors the forwarder, the container's health, and the admin login endpoint.
