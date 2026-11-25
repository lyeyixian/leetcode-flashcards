# Stack

The frameworks and libraries, each with its job. A row marked "Proposed" is not agreed yet.

| Layer | Choice | Job | Status |
|---|---|---|---|
| Language | TypeScript | All app code, in the web app and the Sync API | [ADR-0003](../adr/0003-react-with-vite-and-typescript.md), [ADR-0008](../adr/0008-a-small-sync-api-with-sqlite-on-the-home-server.md) |
| Repo layout | One repo: `web/`, `api/`, `content/`, as pnpm workspaces | Keeps the web app, the API and the content apart | Proposed |
| Content format | Markdown with YAML front matter | Problem cards and pattern pages in `content/` | [ADR-0001](../adr/0001-card-content-as-markdown-in-the-repo.md) |
| Content build | `gray-matter` and `zod`, in a Vite plugin | Reads the Markdown at build time, and fails the build on a card that breaks the format | Proposed |
| UI framework | React | All pages and the session state | [ADR-0003](../adr/0003-react-with-vite-and-typescript.md) |
| Routing | React Router | The pages and the bottom menu | Proposed |
| Build | Vite | Builds the static files of the web app | [ADR-0003](../adr/0003-react-with-vite-and-typescript.md) |
| Offline and install | `vite-plugin-pwa` (Workbox) | The service worker cache and the home screen manifest | Proposed |
| Scheduler | `ts-fsrs` | Calculates the next due date of each card | Proposed |
| Device store | IndexedDB, through Dexie | The rating log and the settings on one device | [ADR-0004](../adr/0004-progress-is-a-log-of-every-rating.md) |
| API framework | Hono on Node.js | The sync routes, and the web app files | Proposed |
| Progress database | SQLite, one file, through `better-sqlite3` | The rating log and the settings of all devices | [ADR-0008](../adr/0008-a-small-sync-api-with-sqlite-on-the-home-server.md) |
| Unit tests | Vitest | The scheduler, the log merge, the content parser and the API routes | Proposed |
| End-to-end tests | Playwright, iPhone viewport | A practice session, offline and online | Proposed |
| Checks | GitHub Actions on each pull request | Type check, lint, unit tests, end-to-end tests and the build must pass | Proposed |
| Release | GitHub Actions, GHCR, a systemd timer | Builds the image on each merge and pulls it to the home server | [ADR-0011](../adr/0011-release-an-image-on-ghcr-that-the-home-server-pulls.md) |
| Runtime | Docker Compose on the home server | Runs the Sync API | [ADR-0008](../adr/0008-a-small-sync-api-with-sqlite-on-the-home-server.md) |
| Access | Tailscale Serve | HTTPS on the tailnet, and the login of each request | [ADR-0009](../adr/0009-reach-the-home-server-through-tailscale-serve.md), [ADR-0010](../adr/0010-the-api-trusts-the-tailscale-identity-of-the-owner.md) |
| Backup | A daily copy of the SQLite file with `sqlite3 .backup` | Keeps the synced progress if the home server disk fails | Proposed |
