# Deployment

This page shows where each part runs. Version 1 has one environment, the home server.

## Deployment: home server

On which machine does each part run, and what connects the machines?

```mermaid
%%{init: {"theme":"base","themeVariables":{"fontFamily":"system-ui, sans-serif","fontSize":"14px","lineColor":"#8b949e","clusterBkg":"#161b22","clusterBorder":"#30363d","edgeLabelBackground":"#0d1117","primaryTextColor":"#e6edf3","titleColor":"#e6edf3"},"flowchart":{"curve":"linear","nodeSpacing":40,"rankSpacing":50}}}%%
flowchart TB
    subgraph phone["iPhone, device"]
        app["<b>Web app</b><br/>[Container: React, Vite]<br/><small>All pages, works offline</small>"]:::container
        local[("<b>Device store</b><br/>[Container: IndexedDB]<br/><small>Rating log of this device</small>")]:::store
    end

    subgraph tailnet["Tailnet, network"]
            ts["<b>Tailscale Serve</b><br/>[External: Tailscale]<br/><small>HTTPS, owner identity</small>"]:::external
    end

    subgraph server["Home server, server"]
        subgraph docker["Docker Compose, runtime"]
            api["<b>Sync API</b><br/>[Container: TypeScript]<br/><small>Stores and returns entries</small>"]:::container
        end
        db[("<b>Progress database</b><br/>[Container: SQLite]<br/><small>Rating log of all devices</small>")]:::store
    end

    app -- "reads, writes" --> local
    app -- "HTTPS" --> ts
    ts -- "HTTP, 127.0.0.1" --> api
    api -- "reads, writes" --> db

    classDef person fill:#1f6feb,stroke:#58a6ff,color:#ffffff,rx:8,ry:8
    classDef system fill:#1f6feb,stroke:#58a6ff,color:#ffffff,rx:8,ry:8
    classDef container fill:#0c2d6b,stroke:#388bfd,color:#e6edf3,rx:8,ry:8
    classDef component fill:#0c2d6b,stroke:#388bfd,color:#e6edf3,rx:8,ry:8
    classDef store fill:#0c2d6b,stroke:#388bfd,color:#e6edf3
    classDef external fill:#21262d,stroke:#8b949e,color:#c9d1d9,stroke-dasharray:4 3,rx:8,ry:8
```

| Node | Runs | Holds |
|---|---|---|
| iPhone | Safari and the home screen app, with the Tailscale app | The web app files in the service worker cache, and the device store |
| Tailnet | Tailscale Serve, with one service and one `ts.net` address for this site | The HTTPS certificate |
| Home server | Ubuntu with Docker Compose. The Sync API is one Compose project, bound to `127.0.0.1` | The progress database file on a Docker volume |

The laptop is a second device with the same parts as the iPhone.

## Settings

| Setting | Where it is set | Purpose |
|---|---|---|
| `OWNER_LOGIN` | The Compose environment on the home server | The Tailscale login that the Sync API accepts |
| `DATABASE_PATH` | The Compose environment on the home server | The path of the SQLite file inside the container (Proposed) |
| `PORT` | The Compose environment on the home server | The port on `127.0.0.1` that Tailscale Serve forwards to (Proposed) |
