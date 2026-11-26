# New problems

New problems are cards for LeetCode problems that you did not attempt yet. They come from popular lists first, then from your company lists. You get one when you ask after you finish your practice session, and you can choose to get one each day.

**As** the person who practises LeetCode on the commute,
**I want** new problems in the order of the popular lists, then my target companies,
**so that** I cover the problems that interviews ask most before random ones.

## Acceptance criteria

### 1. Ask for a new problem

- **Given** I finished all my due cards and new cards for today
- **When** I tap `New problem` on `Practice`
- **Then** I see the front of a card for a problem that I did not attempt
- **And** the card has the tag `new`

### 2. Finish the session first

- **Given** I still have due cards or new cards for today
- **When** I look at `Practice`
- **Then** the `New problem` button is not active
- **And** under it I see the message `Finish today's cards first`

### 3. The popular lists come first

- **Given** I ask for new problems over time
- **When** I get each one
- **Then** it comes from the first list in this order that still has a problem for me: Blind 75, NeetCode 150, NeetCode 250, my company lists
- **And** it is never a problem that I attempted

### 4. Company lists in my order

- **Given** I finished Blind 75, NeetCode 150 and NeetCode 250
- **When** I ask for a new problem
- **Then** it comes from the first company in my company list that still has a problem for me
- **And** the first order of the company list is Jane Street, Google, Stripe, Supabase, TikTok

### 5. Add a company

- **Given** I am on `Company lists` in `Settings`
- **When** I tap `Add company` and pick a company from the list of companies on LeetCode
- **Then** the company is at the end of my company list
- **And** the list of companies to pick from does not show a company that is already in my list

### 6. A new company before its cards are ready

- **Given** I added a company and its cards are not written yet
- **When** I look at `Company lists`
- **Then** the company shows `Cards on the way`
- **And** new problems skip that company until its cards are ready

### 7. Remove a company

- **Given** I am on `Company lists`
- **When** I remove a company
- **Then** the company is not in my list
- **And** new problems do not come from that company
- **And** cards of that company that I already got stay in my practice session

### 8. Reorder the companies

- **Given** I am on `Company lists`
- **When** I move a company up or down the list
- **Then** the next new problem from the company lists follows the new order

### 9. Turn on a daily new problem

- **Given** I am in `Settings`
- **When** I turn on `Daily new problem`
- **Then** each day one new problem is added to my practice session
- **When** I turn it off
- **Then** no new problem is added unless I ask for one

### 10. Daily is off by default

- **Given** I did not change the setting
- **When** I open `Settings`
- **Then** `Daily new problem` is off

### 11. A new problem joins the schedule

- **Given** I rated a new problem card
- **When** it is due again
- **Then** it comes back in my practice session like any other card

### 12. All lists are finished

- **Given** I have a card for every problem on all the lists
- **When** I tap `New problem`
- **Then** I see the message `You finished every list`

## Assumptions

- The cards for all list problems are written in one load before you use them, a few hundred in all. A new problem shows the next card from that set.
- Cards for a company that you add come with the next load. Until then the company shows `Cards on the way`.
- Company lists change over time. A new load before your interviews can bring them up to date.
- Random LeetCode problems after the lists are not in this version. Criterion 12 is my proposal for that point.
- A new problem that you ask for shows at once and is not part of the 5 new cards a day.
- A problem that is on two company lists comes from the first of the two in your order. You get it one time only.
- The labels `Finish today's cards first`, `Company lists`, `Add company` and `Cards on the way` are my proposal.

## Decisions

- **Popular lists before random problems.** You asked on 20 Nov 2026 for Blind 75 and the NeetCode lists first, and random problems only after those.
- **Company lists are in.** You asked for them on 20 Nov 2026.
- **You can add, remove and reorder the companies.** You asked for this on 26 Nov 2026. You can only add a company that is on LeetCode.
- **New problems only after the session.** You chose on 26 Nov 2026 to get a new problem only when you finish your practice session. This replaces "ask any time" from 20 Nov 2026.
- **Daily is a choice, off by default.** You asked on 20 Nov 2026 to choose if one comes each day, and agreed on 26 Nov 2026 that it starts off.
- **The labels `New problem`, `new`, `Daily new problem` and `You finished every list`.** You agreed to these on 26 Nov 2026.
