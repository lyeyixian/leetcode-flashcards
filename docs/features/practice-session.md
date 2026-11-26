# Practice session

A practice session is the main way you use the site. You open `Practice` and the site gives you the cards that are due today, then some new cards. After each card you give a rating, and the rating sets when the card comes back.

**As** the person who practises LeetCode on the commute,
**I want** the cards I am about to forget to come back to me at the right time,
**so that** I remember each pattern without a review of all the cards every day.

## Acceptance criteria

### 1. See what today holds

- **Given** I have cards that are due today and new cards that I did not see yet
- **When** I open `Practice`
- **Then** I see the number of due cards and the number of new cards for today
- **And** I see a `Start` button

### 2. Due cards come first

- **Given** I have due cards and new cards today
- **When** I start the session
- **Then** I get all the due cards before the first new card

### 3. All due cards show

- **Given** I have more due cards today than my number of new cards a day
- **When** I do the session
- **Then** I get every due card
- **And** the number of new cards a day does not limit the due cards

### 4. Five new cards a day by default

- **Given** I did not change the setting
- **When** I finish the due cards
- **Then** I get at most 5 new cards that day

### 5. Change the number of new cards a day

- **Given** I am in `Settings`
- **When** I change `New cards a day` to a different number
- **Then** the next session uses that number of new cards

### 6. New cards mix the patterns

- **Given** I have new cards from more than one pattern
- **When** I get new cards in a session
- **Then** they do not all come from one pattern
- **And** they do not follow the order of the pattern list

### 7. A pattern starts with the problems that teach its template

- **Given** I did not see any card of a pattern yet
- **When** that pattern first gives me new cards
- **Then** I get its first problems, the ones that teach its template, before its other problems
- **And** the other problems of that pattern join the mix after those

### 8. Rate a card

- **Given** I revealed all the steps of a card
- **When** I look at the bottom of the card
- **Then** I see four buttons, `Again`, `Hard`, `Good` and `Easy`
- **And** each button shows its rule under it

The rules are:

- `Again`: wrong pattern, or no idea.
- `Hard`: right pattern, but stuck on the key step or the time and space.
- `Good`: right pattern, approach, and time and space, with some thought.
- `Easy`: all of it, at once.

### 9. Again brings the card back in the same session

- **Given** I rate a card `Again`
- **When** I continue the session
- **Then** I see that card again before the session ends

### 10. Harder cards come back sooner

- **Given** I rate one card `Hard` and a similar card `Easy` on the same day
- **When** I look at when each card is due next
- **Then** the `Hard` card is due before the `Easy` card
- **And** each time I rate a card `Good` or `Easy`, the gap until it is due again is longer than the gap before

### 11. Nothing to do today

- **Given** I have no due cards and no new cards left for today
- **When** I open `Practice`
- **Then** I see the message `All done for today`
- **And** I see the date when the next card is due
- **And** I see the `Pattern quiz` and the `New problem` button as other things to do

### 12. Stop in the middle of a session

- **Given** I rated some cards in a session
- **When** I close the site and open it again later that day
- **Then** the ratings I gave are kept
- **And** the session continues from the next card that I did not rate

## Assumptions

- About 110 cards from the problems you attempted are new cards on the first day. At 5 a day, the first pass takes about 22 days.
- The labels `Start`, `New cards a day` and `All done for today` are my proposal.

## Decisions

- **Spaced repetition.** You asked on 20 Nov 2026 for cards that you saw to come back to you.
- **The schedule follows your past ratings, not fixed gaps.** You agreed to this on 20 Nov 2026 from the research.
- **Due cards always show.** You chose this on 20 Nov 2026, so that due cards do not pile up.
- **5 new cards a day, with a setting.** You chose the default and asked for the setting on 20 Nov 2026.
- **New cards mix the patterns after each pattern's first problems.** You chose this on 20 Nov 2026, over new cards in list order.
- **A rule under each rating button.** You agreed to this on 20 Nov 2026 from the research.
