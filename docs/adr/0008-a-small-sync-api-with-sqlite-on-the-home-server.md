# ADR-0008: A small sync API with SQLite on the home server

**Status:** Accepted (2026-11-25)

## Context

[ADR-0005](0005-supabase-holds-the-synced-progress.md) put the progress in Supabase. Since then, the site moved to the home server ([ADR-0007](0007-host-the-site-on-the-home-server.md)), and the owner wants the sync backend there too. The home server runs Ubuntu with Docker Compose, and serves each app on the tailnet with Tailscale Serve. The rating log ([ADR-0004](0004-progress-is-a-log-of-every-rating.md)) needs only two calls: add entries, and get the entries that a device does not have.

Options: a small API with SQLite on the home server, self-hosted Supabase on the home server, Supabase in the cloud.

## Decision

A small TypeScript API on the home server stores the rating log and the settings in one SQLite file. The API and the web app run in Docker Compose on the home server.

## Consequences

- The ratings sync only when the device can reach the home server. Until then, the entries wait in the device store, so no rating is lost.
- The project now has server code: the API, its tests and its SQLite schema.
- The SQLite file is the only copy of the synced progress. The home server needs a backup of that file.
- Self-hosted Supabase lost because it runs about 10 containers for two calls.
- Supabase in the cloud lost because the owner wants the backend at home. It also pauses after 7 days with no use.
- [ADR-0006](0006-sign-in-with-github-through-supabase-auth.md) used Supabase Auth, so the sign-in decision is open again.
