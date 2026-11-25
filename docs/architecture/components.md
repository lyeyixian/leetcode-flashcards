# Components

This page shows what happens in the main flows. Each container is one simple part, so this page has no level 3 diagram.

## Sequence: rate a card

What happens when the person rates a card, with or without a signal?

```mermaid
sequenceDiagram
    autonumber
    actor User as Yi Xian
    participant App as Web app
    participant Local as Device store
    participant TS as Tailscale Serve
    participant API as Sync API

    User->>App: taps a rating
    App->>Local: adds a log entry, not synced
    App-->>User: shows the next card
    App->>TS: sends the entries that are not synced
    alt no signal, or Tailscale is off
        App->>App: keeps the entries, tries again later
    else the request reaches the home server
        TS->>API: forwards with Tailscale-User-Login
        alt the login is not OWNER_LOGIN
            API-->>App: 403
            App->>App: keeps the entries, shows a sync error
        else the login is OWNER_LOGIN
            API->>API: adds the entries it does not have
            API-->>App: entries from the other devices
            App->>Local: marks the sent entries as synced, adds the new entries
            App->>App: calculates the state of each card again
        end
    end
```
