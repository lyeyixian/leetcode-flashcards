# ADR-0005: Supabase holds the synced progress

**Status:** Accepted (2026-11-25)

## Context

The rating log ([ADR-0004](0004-progress-is-a-log-of-every-rating.md)) needs a store that all devices can reach. The store only adds entries and returns the entries that a device does not have. One person uses the site, so the data is a few thousand rows. The project has no budget for a paid plan.

Options: a Cloudflare Worker with D1, Supabase, Firebase Firestore.

## Decision

A Supabase project on the free plan holds the progress in Postgres. The web app reads and writes the tables directly with `supabase-js`. The project has no server code of its own.

## Consequences

- The tables are `review_log` (one row for each rating) and `settings` (one row for each setting). Row level security limits each row to its owner.
- The site needs no API of its own, so the repo has only client code and SQL migrations.
- The free plan pauses a project after 7 days with no requests. After a long break, the owner must restore the project in the Supabase dashboard before the sync works again. The site keeps working offline while the project is paused.
- The Supabase URL and the anon key are in the web app, which is public. Row level security is the only guard on the data.
- A Cloudflare Worker with D1 lost because it needs an API that the project writes and tests.
- Firestore lost because its own offline cache repeats the rating log.
