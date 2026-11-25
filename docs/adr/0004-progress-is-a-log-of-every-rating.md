# ADR-0004: Progress is a log of every rating

**Status:** Accepted (2026-11-25)

## Context

The feature spec says that a rating made with no signal must reach every device, and that no rating is lost. The person can rate cards on a phone and on a laptop, and both can be offline at the same time. FSRS ([ADR-0002](0002-fsrs-schedules-the-reviews.md)) can calculate the state of a card from its list of ratings.

Options: a log of every rating, the latest state for each card, a ready-made sync engine such as RxDB or PouchDB.

## Decision

Each rating is one entry in an append-only log. An entry has a random ID, the card, the rating, the time and the device. The web app saves each entry in IndexedDB first, then sends the entries that are not synced yet. The web app calculates the state of each card from the log. Each setting keeps only its latest value, and the newest change wins.

## Consequences

- To merge two devices, the sync adds the entries that a device does not have. Two entries never conflict, so no rating is lost.
- The sync service only stores and returns entries. It has no scheduling logic.
- The latest state for each card lost because two offline ratings of one card overwrite each other.
- A ready-made sync engine lost because it fixes the choice of backend.
- The web app replays the log at start. A few thousand entries take a few milliseconds, but the log only grows.
- A rating cannot be edited or removed. A wrong tap stays in the log.
