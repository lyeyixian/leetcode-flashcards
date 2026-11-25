# Stack

The frameworks and libraries, each with its job. A row marked "Proposed" is not agreed yet.

| Layer | Choice | Job | Status |
|---|---|---|---|
| Language | TypeScript | All app code, in the web app and the Sync API | [ADR-0003](../adr/0003-react-with-vite-and-typescript.md), [ADR-0008](../adr/0008-a-small-sync-api-with-sqlite-on-the-home-server.md) |
| Content format | Markdown with YAML front matter | Problem cards and pattern pages in `content/` | [ADR-0001](../adr/0001-card-content-as-markdown-in-the-repo.md) |
| UI framework | React | All pages and the session state | [ADR-0003](../adr/0003-react-with-vite-and-typescript.md) |
| Build | Vite | Builds the static files of the web app | [ADR-0003](../adr/0003-react-with-vite-and-typescript.md) |
| Scheduler | `ts-fsrs` | Calculates the next due date of each card | Proposed |
| Device store | IndexedDB | The rating log and the settings on one device | [ADR-0004](../adr/0004-progress-is-a-log-of-every-rating.md) |
| Progress database | SQLite, one file | The rating log and the settings of all devices | [ADR-0008](../adr/0008-a-small-sync-api-with-sqlite-on-the-home-server.md) |
| Runtime | Docker Compose on the home server | Runs the Sync API | [ADR-0008](../adr/0008-a-small-sync-api-with-sqlite-on-the-home-server.md) |
| Access | Tailscale Serve | HTTPS on the tailnet, and the login of each request | [ADR-0009](../adr/0009-reach-the-home-server-through-tailscale-serve.md), [ADR-0010](../adr/0010-the-api-trusts-the-tailscale-identity-of-the-owner.md) |
