# Company lists

Company lists are the companies that you target, in the order that you want their problems. You find them under `Company lists` in `Settings`. You use them to choose which company problems come to you after the popular lists.

**As** the person who practises LeetCode on the commute,
**I want** to choose and order the companies I target,
**so that** my new problems follow the interviews I have coming.

## Acceptance criteria

### 1. See my company list

- **Given** I did not change my company list
- **When** I open `Company lists` in `Settings`
- **Then** I see Jane Street, Google, Stripe, Supabase and TikTok, in that order

### 2. Add a company

- **Given** I am on `Company lists` in `Settings`
- **When** I tap `Add company` and pick a company from the list of companies on LeetCode
- **Then** the company is at the end of my company list
- **And** the list of companies to pick from does not show a company that is already in my list

### 3. A new company before its cards are ready

- **Given** I added a company and its cards are not written yet
- **When** I look at `Company lists`
- **Then** the company shows `Cards on the way`
- **And** new problems skip that company until its cards are ready

### 4. Remove a company

- **Given** I am on `Company lists`
- **When** I remove a company
- **Then** the company is not in my list
- **And** new problems do not come from that company
- **And** cards of that company that I already got stay in my practice session

### 5. Reorder the companies

- **Given** I am on `Company lists`
- **When** I move a company up or down the list
- **Then** the next new problem from the company lists follows the new order

## Assumptions

- The labels `Company lists`, `Add company` and `Cards on the way` are my proposal.

## Decisions

- **You can add, remove and reorder the companies.** You asked for this on 26 Nov 2026. You can only add a company that is on LeetCode.
- **A new company waits for the next load.** You agreed on 26 Nov 2026 that a company you add shows `Cards on the way` until its cards come with the next load. The site does not write cards while you use it.
- **Company lists are their own feature.** You chose this on 26 Nov 2026.
