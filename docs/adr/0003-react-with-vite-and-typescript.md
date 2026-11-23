# ADR-0003: React with Vite and TypeScript

**Status:** Accepted (2026-11-23)

## Context

Almost every page of the site is interactive: the practice session, the step reveals, the decision tree and the drill. The site must work with no signal and install to the home screen. The state of a practice session continues across pages. The site has no page that needs server rendering.

Options: React with Vite, Svelte with SvelteKit, Astro with interactive islands.

## Decision

The site is a single-page app in React and TypeScript, built with Vite. The build output is static files.

## Consequences

- React has the most examples and libraries for offline apps, IndexedDB and FSRS. This decided the choice.
- One client state holds the session across pages. Astro islands lost because they make shared state across pages hard.
- Any static host can serve the build. The choice of host stays open.
- The React runtime adds about 45 KB (gzip) to the first load. Svelte makes a smaller bundle.
