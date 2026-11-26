# Pattern decision tree

The pattern decision tree is a set of yes or no questions about a problem. Each answer narrows the choice until one pattern is left. You see the full tree on the patterns page, and you can walk it on a card to pick the pattern.

**As** the person who practises LeetCode on the commute,
**I want** a fixed set of questions that lead me to the right pattern,
**so that** I can name the pattern of a new problem in less than 2 minutes.

## Acceptance criteria

### 1. See the full tree

- **Given** I am on `Patterns`
- **When** I open the decision tree
- **Then** I see every question and every pattern that the questions lead to
- **And** each pattern in the tree links to its pattern page

### 2. Walk the tree on a card

- **Given** I am at the Pattern step of a card
- **When** I choose to walk the tree
- **Then** I see one question at a time, such as "Contiguous subarray?"
- **And** I answer each question with `Yes` or `No`

### 3. Land on a pattern

- **Given** I am walking the tree on a card
- **When** I answer the last question on my path
- **Then** I see the pattern that my answers lead to, as my pick

### 4. See where I went wrong

- **Given** my answers lead to the wrong pattern
- **When** the Pattern step opens
- **Then** I see the first question that I answered wrong
- **And** I see the right answer to that question for this problem

### 5. Go back one question

- **Given** I am walking the tree on a card
- **When** I tap `Back`
- **Then** I see the question before, and I can change my answer

## Assumptions

- Each card holds the right path through the tree for its problem, so the site can show the first wrong answer.
- The labels `Yes`, `No` and `Back` are my proposal.

## Decisions

- **The tree is on the patterns page and on each card.** You asked for it on 20 Nov 2026 from the research.
