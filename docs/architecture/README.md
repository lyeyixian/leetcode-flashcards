# Architecture

These pages show how the LeetCode flashcards site is built. They describe version 1, the site in the [feature spec](../features/README.md). The reasons for each choice are in the [ADRs](../adr/README.md).

| Page | What it shows |
|---|---|
| This page | C4 level 1 and 2: the system and its containers |
| [Stack](stack.md) | The frameworks and libraries, each with its job |

## Level 1: system context

Who uses the site, and which outside systems does it depend on?

```mermaid
%%{init: {"theme":"base","themeVariables":{"fontFamily":"system-ui, sans-serif","fontSize":"14px","lineColor":"#8b949e","clusterBkg":"#161b22","clusterBorder":"#30363d","edgeLabelBackground":"#0d1117","primaryTextColor":"#e6edf3","titleColor":"#e6edf3"},"flowchart":{"curve":"linear","nodeSpacing":40,"rankSpacing":50}}}%%
flowchart TB
    user(["<b>Yi Xian</b><br/>[Person]<br/><small>Practises on the commute</small>"]):::person
    system["<b>LeetCode flashcards</b><br/>[Software system]<br/><small>Cards, schedule, patterns</small>"]:::system
    leetcode["<b>LeetCode</b><br/>[External: website]<br/><small>Full problem pages</small>"]:::external
    github["<b>GitHub</b><br/>[External: OAuth]<br/><small>Proves who signs in</small>"]:::external

    user -- "practises, HTTPS" --> system
    system -- "signs in, OAuth" --> github
    user -- "opens a problem" --> leetcode
    system -. "links to" .-> leetcode

    classDef person fill:#1f6feb,stroke:#58a6ff,color:#ffffff,rx:8,ry:8
    classDef system fill:#1f6feb,stroke:#58a6ff,color:#ffffff,rx:8,ry:8
    classDef container fill:#0c2d6b,stroke:#388bfd,color:#e6edf3,rx:8,ry:8
    classDef component fill:#0c2d6b,stroke:#388bfd,color:#e6edf3,rx:8,ry:8
    classDef store fill:#0c2d6b,stroke:#388bfd,color:#e6edf3
    classDef external fill:#21262d,stroke:#8b949e,color:#c9d1d9,stroke-dasharray:4 3,rx:8,ry:8
```

| Element | Kind | Job |
|---|---|---|
| Yi Xian | Person | The only user. Practises cards on a phone, and sometimes on a laptop. |
| LeetCode flashcards | This system | Shows the cards and the pattern pages, schedules the reviews, and keeps the progress the same on all devices. |
| LeetCode | Outside system | Shows the full problem. The site only links to it. |
| GitHub | Outside system | Proves that the person who signs in is the owner. See [ADR-0006](../adr/0006-sign-in-with-github-through-supabase-auth.md). |

## Level 2: containers

Which parts make up the site, and how do they talk?

```mermaid
%%{init: {"theme":"base","themeVariables":{"fontFamily":"system-ui, sans-serif","fontSize":"14px","lineColor":"#8b949e","clusterBkg":"#161b22","clusterBorder":"#30363d","edgeLabelBackground":"#0d1117","primaryTextColor":"#e6edf3","titleColor":"#e6edf3"},"flowchart":{"curve":"linear","nodeSpacing":40,"rankSpacing":50}}}%%
flowchart TB
    user(["<b>Yi Xian</b><br/>[Person]<br/><small>Practises on the commute</small>"]):::person

    subgraph system["LeetCode flashcards"]
        content[("<b>Card content</b><br/>[Container: Markdown]<br/><small>Cards and pattern pages</small>")]:::store
        app["<b>Web app</b><br/>[Container: React, Vite]<br/><small>All pages, works offline</small>"]:::container
        local[("<b>Device store</b><br/>[Container: IndexedDB]<br/><small>Rating log of this device</small>")]:::store
        db[("<b>Progress database</b><br/>[Container: Supabase]<br/><small>Rating log of all devices</small>")]:::store
    end

    github["<b>GitHub</b><br/>[External: OAuth]<br/><small>Proves who signs in</small>"]:::external

    user -- "taps, HTTPS" --> app
    content -. "built into" .-> app
    app -- "reads, writes" --> local
    app -- "syncs, HTTPS" --> db
    db -- "checks sign-in" --> github

    classDef person fill:#1f6feb,stroke:#58a6ff,color:#ffffff,rx:8,ry:8
    classDef system fill:#1f6feb,stroke:#58a6ff,color:#ffffff,rx:8,ry:8
    classDef container fill:#0c2d6b,stroke:#388bfd,color:#e6edf3,rx:8,ry:8
    classDef component fill:#0c2d6b,stroke:#388bfd,color:#e6edf3,rx:8,ry:8
    classDef store fill:#0c2d6b,stroke:#388bfd,color:#e6edf3
    classDef external fill:#21262d,stroke:#8b949e,color:#c9d1d9,stroke-dasharray:4 3,rx:8,ry:8
```

| Container | Technology | Job |
|---|---|---|
| Card content | Markdown files in `content/` | Holds the problem cards and the pattern pages. The build turns them into data for the web app. See [ADR-0001](../adr/0001-card-content-as-markdown-in-the-repo.md). |
| Web app | React, TypeScript, Vite | Shows all pages, runs the practice session and the drill, and schedules each card. It runs in the phone browser and works with no signal. See [ADR-0003](../adr/0003-react-with-vite-and-typescript.md). |
| Device store | IndexedDB in the browser | Holds the rating log and the settings on one device, and marks each entry that is not synced yet. A session works with no signal. See [ADR-0004](../adr/0004-progress-is-a-log-of-every-rating.md). |
| Progress database | Supabase Postgres, free plan | Holds the rating log in `review_log` and the settings in `settings`, for all devices. The web app calls it directly with `supabase-js`. Supabase Auth signs the person in with GitHub, and row level security limits each row to that account. See [ADR-0005](../adr/0005-supabase-holds-the-synced-progress.md) and [ADR-0006](../adr/0006-sign-in-with-github-through-supabase-auth.md). |

## Sequence: rate a card

What happens when the person rates a card, with or without a signal?

```mermaid
sequenceDiagram
    autonumber
    actor User as Yi Xian
    participant App as Web app
    participant Local as Device store
    participant DB as Progress database

    User->>App: taps a rating
    App->>Local: adds a log entry, not synced
    App-->>User: shows the next card
    App->>DB: sends the entries that are not synced
    alt no signal, the request fails
        App->>App: keeps the entries, tries again later
    else the sync works
        DB-->>App: entries from the other devices
        App->>Local: marks the sent entries as synced, adds the new entries
        App->>App: calculates the state of each card again
    end
```

## What stays out of this repo

- The progress data. The repo is public, so the ratings live only in the progress database and on each device.
- The Supabase service role key. Only the Supabase dashboard and the migration step use it. The anon key is public by design and can be in the web app.
