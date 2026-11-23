# Pattern drill

The pattern drill is a fast second mode. It shows only the problem and asks you to pick its pattern from 4 choices. Each card takes about 10 seconds, so you can do many on a short trip.

**As** the person who practises LeetCode on the commute,
**I want** to name the pattern of many problems quickly,
**so that** I can recognise a pattern in an interview in less than 2 minutes.

## Acceptance criteria

### 1. Start a drill

- **Given** I am on any page
- **When** I open `Drill`
- **Then** I see the front of a card and 4 pattern choices under it

### 2. Pick a pattern

- **Given** I see a drill card
- **When** I tap a pattern
- **Then** I see if my pick was right
- **And** I see the right pattern and its one-line key

### 3. Go to the next card

- **Given** I picked a pattern on a drill card
- **When** I tap `Next`
- **Then** I see a different card and 4 new choices

### 4. See my score

- **Given** I picked a pattern on some drill cards
- **When** I end the drill
- **Then** I see how many I got right out of how many I tried

### 5. Drill picks only use cards I saw

- **Given** I start a drill
- **When** the site picks a card
- **Then** the card is one that I already saw in a practice session

### 6. The drill does not change the schedule

- **Given** I did a drill
- **When** I open `Practice`
- **Then** the due cards for today are the same as before the drill

### 7. Nothing to drill

- **Given** I did not see any card in a practice session yet
- **When** I open `Drill`
- **Then** I see the message `Practise some cards first`

## Assumptions

- Criteria 5, 6 and 7 are my choices. A drill on cards you never saw tests guessing, not recall.
- The labels `Drill`, `Next` and `Practise some cards first` are my proposal.

## Decisions

- **The drill is in.** You chose this on 20 Nov 2026.
