# ADR-0006: Sign in with GitHub through Supabase Auth

**Status:** Superseded by [ADR-0010](0010-the-api-trusts-the-tailscale-identity-of-the-owner.md) (2026-11-25)

## Context

The Supabase anon key is public in the web app ([ADR-0005](0005-supabase-holds-the-synced-progress.md)), so row level security must know who makes each request. One person uses the site. The feature spec says that no page asks the person to type. The phone is an iPhone, and a home screen app on iOS keeps its storage apart from Safari.

Options: sign in with GitHub, an email magic link, a sync code shared by QR code.

## Decision

The web app signs in with the GitHub provider of Supabase Auth, one time on each device. The sign-in opens in the same window, not in a popup. After the first account exists, new sign-ups are turned off in Supabase. Each row in `review_log` and `settings` has a `user_id`, and row level security allows only `auth.uid() = user_id`.

## Consequences

- Sign-in needs one tap on a device where GitHub is already signed in. No page asks for an email.
- No other person can make an account, so no other person can read or write the progress.
- The sign-in must be tested in the home screen app on the iPhone before other work builds on it. If the session does not reach the home screen app, this decision is open again.
- A practice session works before sign-in. The rating log stays on the device and syncs after sign-in.
- The magic link lost because the email link opens in Safari, not in the home screen app.
- The QR code lost because any person with the code can write to the progress.
- The site depends on GitHub for sign-in. A GitHub outage stops a new sign-in, but not a session that is already signed in.
