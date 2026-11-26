# Desk list

The desk list is a weekly list of problems to code at a desk. It holds the cards that you rated `Again` or `Hard` that week. You use it to turn what you recall on the train into code that you write and run.

**As** the person who practises LeetCode on the commute,
**I want** a list of the problems I struggled with this week,
**so that** I can code them at a desk and fix the gaps.

## Acceptance criteria

### 1. See this week's list

- **Given** I rated some cards `Again` or `Hard` this week
- **When** I open `Desk list` from the menu
- **Then** I see each of those problems one time, with its number, title and pattern
- **And** the problems I rated `Again` come before the ones I rated only `Hard`

### 2. Open a problem from the list

- **Given** I am on the desk list
- **When** I tap a problem
- **Then** the LeetCode page of the problem opens in a new tab

### 3. Mark a problem as coded

- **Given** I am on the desk list
- **When** I tick a problem
- **Then** the problem shows as done
- **When** I tick it again
- **Then** it shows as not done

### 4. A new list each week

- **Given** a new week starts on Monday
- **When** I open the desk list
- **Then** I see only the cards that I rated `Again` or `Hard` since that Monday

### 5. Nothing to code this week

- **Given** I did not rate any card `Again` or `Hard` this week
- **When** I open the desk list
- **Then** I see the message `Nothing to code this week`

## Assumptions

- The label `Nothing to code this week` is my proposal.

## Decisions

- **A weekly desk list.** You agreed to this on 20 Nov 2026 from the research.
- **The week starts on Monday.** You agreed to this on 26 Nov 2026.
- **The desk list has its own page.** You chose this on 26 Nov 2026, over a link inside `Practice`.
