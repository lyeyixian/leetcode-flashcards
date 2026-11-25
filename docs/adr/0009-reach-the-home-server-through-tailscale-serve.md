# ADR-0009: Reach the home server through Tailscale Serve

**Status:** Accepted (2026-11-25)

## Context

The web app and the sync API run on the home server ([ADR-0007](0007-host-the-site-on-the-home-server.md), [ADR-0008](0008-a-small-sync-api-with-sqlite-on-the-home-server.md)). The iPhone must reach them from outside the home over HTTPS with a trusted certificate, or the service worker and the home screen app do not work. The owner already uses Tailscale, and the home server serves its other apps on the tailnet with Tailscale Serve.

Options: Tailscale with HTTPS, Cloudflare Tunnel, an open port with Caddy.

## Decision

The web app and the sync API have one Tailscale Serve service on the home server, with its own `ts.net` address and the Tailscale HTTPS certificate. The service is on the tailnet only. Tailscale Funnel stays off.

## Consequences

- No port is open to the internet, and the project needs no domain.
- The iPhone must have Tailscale on to install the site, to get a new version, and to sync. Practice works with Tailscale off.
- Only devices on the tailnet can reach the API.
- Cloudflare Tunnel lost because it needs a domain and makes the site public.
- An open port with Caddy lost because it shows the home IP address to the internet.
