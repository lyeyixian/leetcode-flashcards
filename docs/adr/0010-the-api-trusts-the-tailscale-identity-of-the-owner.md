# ADR-0010: The API trusts the Tailscale identity of the owner

**Status:** Accepted (2026-11-25)

## Context

[ADR-0006](0006-sign-in-with-github-through-supabase-auth.md) used Supabase Auth, which the project no longer has ([ADR-0008](0008-a-small-sync-api-with-sqlite-on-the-home-server.md)). Only devices on the tailnet can reach the API ([ADR-0009](0009-reach-the-home-server-through-tailscale-serve.md)). The tailnet can hold devices of other people. Tailscale Serve adds the `Tailscale-User-Login` header to each request from a tailnet user. The feature spec says that no page asks the person to type.

Options: the Tailscale identity, GitHub sign-in in the API, the tailnet only with no check.

## Decision

The site has no sign-in screen. The API listens on `127.0.0.1` only, so each request comes through Tailscale Serve. The API accepts a request only if `Tailscale-User-Login` is the login in the `OWNER_LOGIN` setting. Otherwise it returns `403`.

## Consequences

- The API has no login code, no sessions and no passwords.
- A device of a different person on the tailnet gets `403` and cannot read or write the progress.
- If the API listens on a different address than `127.0.0.1`, a client can send a false header. The bind address is part of this decision.
- A tagged device has no user login, so it cannot sync.
- GitHub sign-in lost because it adds OAuth code and an OAuth app, and Tailscale is already the only way in.
- No check lost because any device on the tailnet could change the progress.
