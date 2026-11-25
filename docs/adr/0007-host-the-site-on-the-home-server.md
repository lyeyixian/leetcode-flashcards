# ADR-0007: Host the site on the home server

**Status:** Accepted (2026-11-25)

## Context

The web app is static files ([ADR-0003](0003-react-with-vite-and-typescript.md)), and the progress syncs to Supabase, not to the host ([ADR-0005](0005-supabase-holds-the-synced-progress.md)). The owner has a home server. The iPhone must load the site over HTTPS with a trusted certificate, or the service worker and the home screen app do not work.

Options: the home server, Cloudflare Pages, GitHub Pages, Vercel.

## Decision

The home server serves the built files of the web app. How the iPhone reaches the home server from outside the home is a separate decision.

## Consequences

- The iPhone needs the home server only to install the site and to get a new version. Practice and sync work when the home server is down.
- The home server needs a trusted HTTPS certificate for the address that the iPhone uses.
- The Supabase Auth redirect URL must list that address.
- No host builds a preview for each pull request. A change shows on the iPhone only after it is released to the home server.
- The owner keeps the home server running and up to date.
