# ADR-0011: Release an image on GHCR that the home server pulls

**Status:** Accepted (2026-11-25)

## Context

The Sync API and the web app files run in Docker Compose on the home server ([ADR-0008](0008-a-small-sync-api-with-sqlite-on-the-home-server.md)). The home server is on the tailnet only ([ADR-0009](0009-reach-the-home-server-through-tailscale-serve.md)), so GitHub cannot reach it without a key to the tailnet. The other apps on the home server already run from images on `ghcr.io`.

Options: an image on GHCR that the server pulls, a GitHub Action that deploys over Tailscale, a build on the server from git.

## Decision

On each merge to `main`, a GitHub Action builds one image and pushes it to `ghcr.io/lyeyixian/leetcode-flashcards` with the tags `latest` and the commit SHA. A systemd timer on the home server runs `docker compose pull` and `docker compose up -d` every 15 minutes.

## Consequences

- GitHub holds no key to the tailnet or to the home server.
- A merged change reaches the home server in 15 minutes or less. The iPhone gets the new version the next time it opens the site with Tailscale on.
- A failed build stops in GitHub Actions, and the home server keeps the old image.
- To roll back, set the image tag in the Compose file to an older commit SHA.
- The image is public, because the repo is public. The image holds no progress data and no setting values.
- The deploy over Tailscale lost because GitHub would hold a Tailscale key and an SSH key.
- The build on the server lost because a failed build is only visible on the server.
