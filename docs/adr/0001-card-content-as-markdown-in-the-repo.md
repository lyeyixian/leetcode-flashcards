# ADR-0001: Card content as Markdown files in the repo

**Status:** Accepted (2026-11-23)

## Context

The site shows a few hundred problem cards and 17 pattern pages. One person writes and edits this content, and the content changes less often than the code. The card format has fixed fields (number, title, difficulty, pattern, template) and a fixed list of steps.

## Decision

Each pattern is one Markdown file in `content/patterns/`, and each problem card is one Markdown file in `content/problems/`. YAML front matter holds the fixed fields. Each step of the card back is one `##` section, in the order of the feature spec.

## Consequences

- A change to a card is a git commit and a pull request. The content has history and review with no extra tool.
- The site must read the Markdown at build time. A content error must fail the build, not show a broken card.
- Card content is public, because the repo is public.
- The format is agreed for now. A later change to the format changes every card file.
