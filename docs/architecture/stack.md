# Stack

The frameworks and libraries, each with its job. A row marked "Proposed" is not agreed yet.

| Layer | Choice | Job | Status |
|---|---|---|---|
| Language | TypeScript | All app code | [ADR-0003](../adr/0003-react-with-vite-and-typescript.md) |
| Content format | Markdown with YAML front matter | Problem cards and pattern pages in `content/` | [ADR-0001](../adr/0001-card-content-as-markdown-in-the-repo.md) |
| UI framework | React | All pages and the session state | [ADR-0003](../adr/0003-react-with-vite-and-typescript.md) |
| Build | Vite | Builds the static files of the web app | [ADR-0003](../adr/0003-react-with-vite-and-typescript.md) |
| Scheduler | `ts-fsrs` | Calculates the next due date of each card | Proposed |
| Device store | IndexedDB | Ratings and settings on one device | Proposed |
