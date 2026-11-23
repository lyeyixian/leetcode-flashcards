# New problems

New problems are cards for LeetCode problems that you did not attempt yet. They come from popular lists first, then from company lists. You get one when you ask, and you can choose to get one each day.

**As** the person who practises LeetCode on the commute,
**I want** new problems in the order of the popular lists,
**so that** I cover the problems that interviews ask most before random ones.

## Acceptance criteria

### 1. Ask for a new problem

- **Given** I am on `Practice`
- **When** I tap `New problem`
- **Then** I see the front of a card for a problem that I did not attempt
- **And** the card has the tag `new`

### 2. The popular lists come first

- **Given** I ask for new problems over time
- **When** I get each one
- **Then** it comes from the first list in this order that still has a problem for me: Blind 75, NeetCode 150, NeetCode 250, my company lists
- **And** it is never a problem that I attempted

### 3. Company lists in order

- **Given** I finished Blind 75, NeetCode 150 and NeetCode 250
- **When** I ask for a new problem
- **Then** it comes from the company lists in this order: Jane Street, Google, Stripe, Supabase, TikTok

### 4. Turn on a daily new problem

- **Given** I am in `Settings`
- **When** I turn on `Daily new problem`
- **Then** each day one new problem is added to my practice session
- **When** I turn it off
- **Then** no new problem is added unless I ask for one

### 5. Daily is off by default

- **Given** I did not change the setting
- **When** I open `Settings`
- **Then** `Daily new problem` is off

### 6. A new problem joins the schedule

- **Given** I rated a new problem card
- **When** it is due again
- **Then** it comes back in my practice session like any other card

### 7. All lists are finished

- **Given** I have a card for every problem on all the lists
- **When** I tap `New problem`
- **Then** I see the message `You finished every list`

## Assumptions

- The cards for all list problems are written in one load before you use them, a few hundred in all. A new problem shows the next card from that set.
- Company lists change over time. A new load before your interviews can bring them up to date.
- Random LeetCode problems after the lists are not in this version. Criterion 7 is my proposal for that point.
- `Daily new problem` off by default is my choice.
- A new problem that you ask for shows at once and is not part of the 5 new cards a day.
- The labels `New problem`, `new`, `Daily new problem` and `You finished every list` are my proposal.

## Decisions

- **Popular lists before random problems.** You asked on 20 Nov 2026 for Blind 75 and the NeetCode lists first, and random problems only after those.
- **Company lists are in.** You asked for them on 20 Nov 2026.
- **Ask any time, daily is a choice.** You asked on 20 Nov 2026 to always be able to ask for one, and to choose if one comes each day.
