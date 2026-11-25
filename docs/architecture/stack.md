# Stack

The frameworks and libraries, each with its job. A row marked "Proposed" is not agreed yet.

| Layer | Choice | Job | Status |
|---|---|---|---|
| Language | TypeScript | All app code | [ADR-0003](../adr/0003-react-with-vite-and-typescript.md) |
| Content format | Markdown with YAML front matter | Problem cards and pattern pages in `content/` | [ADR-0001](../adr/0001-card-content-as-markdown-in-the-repo.md) |
| UI framework | React | All pages and the session state | [ADR-0003](../adr/0003-react-with-vite-and-typescript.md) |
| Build | Vite | Builds the static files of the web app | [ADR-0003](../adr/0003-react-with-vite-and-typescript.md) |
| Scheduler | `ts-fsrs` | Calculates the next due date of each card | Proposed |
| Device store | IndexedDB | The rating log and the settings on one device | [ADR-0004](../adr/0004-progress-is-a-log-of-every-rating.md) |
| Sync client | `supabase-js` | Sends and gets the rating log entries | [ADR-0005](../adr/0005-supabase-holds-the-synced-progress.md) |
| Progress database | Supabase Postgres, free plan | The rating log and the settings of all devices | [ADR-0005](../adr/0005-supabase-holds-the-synced-progress.md) |
