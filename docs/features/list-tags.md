# List tags

List tags show which lists a problem is on. A tag is on the front of each card, such as `Blind 75` or `Google`. You use the tags to see how often interviews ask a problem, and which of your target companies ask it.

**As** the person who practises LeetCode on the commute,
**I want** to see which lists and companies a problem comes from,
**so that** I know which problems matter most for the companies I target.

## Acceptance criteria

### 1. See the lists of a problem

- **Given** a problem is on Blind 75, a NeetCode list or a company in my company list
- **When** I look at the front of its card
- **Then** I see a tag for each of those lists, such as `Blind 75` or `Google`

### 2. Only my companies

- **Given** a problem is on the list of a company that is not in my company list
- **When** I look at the front of its card
- **Then** I see no tag for that company

### 3. A removed company

- **Given** I removed a company from my company list
- **When** I look at the front of a card of that company
- **Then** the tag of that company is not there

### 4. A problem on no list

- **Given** a problem is on no list
- **When** I look at the front of its card
- **Then** I see no list tags

## Decisions

- **List tags are their own feature.** You chose this on 26 Nov 2026.
- **The tags are on the front.** You agreed to this on 26 Nov 2026. A list name does not give away the pattern.
