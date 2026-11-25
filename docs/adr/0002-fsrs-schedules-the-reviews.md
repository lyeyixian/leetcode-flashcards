# ADR-0002: FSRS schedules the reviews

**Status:** Proposed (2026-11-23)

## Context

The practice session shows each card again when it is due. The feature spec says that the schedule follows the past ratings of each card, not fixed gaps. The ratings are `Again`, `Hard`, `Good` and `Easy`. The research for the spec compared two schedulers.

Options: FSRS, SM-2.

## Decision

The site schedules each card with FSRS (Free Spaced Repetition Scheduler), through the `ts-fsrs` library, with a target retention of 90%.

## Consequences

- FSRS uses the four ratings as they are. No rating needs a conversion.
- FSRS gets about the same retention as SM-2 from fewer reviews. This decides the choice, because the commute limits the time to practise.
- Version one uses the default FSRS weights. The weights can later be fitted to the ratings of this one person. That fit needs every past rating, not only the current state of each card.
- FSRS is harder to explain than SM-2. A due date can look wrong to a person who expects fixed gaps.
