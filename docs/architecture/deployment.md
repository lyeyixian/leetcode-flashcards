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

## Sequence: a release

What happens between a merge to `main` and the new version on the iPhone? See [ADR-0011](../adr/0011-release-an-image-on-ghcr-that-the-home-server-pulls.md).

```mermaid
sequenceDiagram
    autonumber
    actor Owner as Yi Xian
    participant GH as GitHub Actions
    participant GHCR as GHCR
    participant Server as Home server
    participant App as Web app

    Owner->>GH: merges a pull request to main
    GH->>GH: runs the checks and builds the image
    alt the checks or the build fail
        GH-->>Owner: shows the failed run
    else the build works
        GH->>GHCR: pushes the image, tags latest and the SHA
        Server->>GHCR: pulls latest, on the 15 minute timer
        alt the image did not change
            GHCR-->>Server: no new image
        else a new image
            GHCR-->>Server: the new image
            Server->>Server: restarts the Sync API container
            App->>Server: gets the new files when opened with Tailscale on
        end
    end
```

## Settings

| Setting | Where it is set | Purpose |
|---|---|---|
| `OWNER_LOGIN` | The Compose environment on the home server | The Tailscale login that the Sync API accepts |
| `DATABASE_PATH` | The Compose environment on the home server | The path of the SQLite file inside the container (Proposed) |
| `IMAGE_TAG` | The Compose file on the home server | The image tag to run. `latest`, or a commit SHA to roll back (Proposed) |
| `PORT` | The Compose environment on the home server | The port on `127.0.0.1` that Tailscale Serve forwards to (Proposed) |

## One-time setup

These steps prepare the home server. They are a proposal until the first release.

1. Make a Compose project for the site on the home server, with a volume for the progress database.
2. Set `OWNER_LOGIN` to the Tailscale login of the owner.
3. Bind the Sync API to `127.0.0.1` on a free port.
4. Add a Tailscale Serve service for the site that forwards HTTPS to that port.
5. Add the systemd timer that pulls the image every 15 minutes.
6. Add a daily backup of the SQLite file.
