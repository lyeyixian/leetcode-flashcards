# Flashcard

A flashcard shows one LeetCode problem on the front. The back holds the steps that you follow in an interview. You reveal the steps one at a time, and you say each step in your head before you reveal it.

**As** the person who practises LeetCode on the commute,
**I want** to recall each interview step before I see it,
**so that** I can talk through a problem in a real interview without notes.

## Acceptance criteria

### 1. Read the front

- **Given** a card is on the screen
- **When** I look at the front
- **Then** I see the problem number, the title and the difficulty
- **And** I see the ask in one or two lines, one small example and the constraints in one line
- **And** I do not see the pattern name anywhere on the front

### 2. Commit to the pattern first

- **Given** I am on the front of a card
- **When** I continue to the back
- **Then** the first step asks me to pick the pattern
- **And** I can pick it from 4 choices, or walk the pattern decision tree
- **And** the 4 choices hold the right pattern and 3 patterns that look like it, such as two pointers for a sliding window problem

### 3. See if my pattern pick was right

- **Given** I picked a pattern
- **When** the Pattern step opens
- **Then** I see the pattern name, the template and the one-line key
- **And** I see if my pick was right or wrong

### 4. Reveal the steps one at a time

- **Given** the Pattern step is open
- **When** I tap `I have it` on each next step
- **Then** the steps open one at a time in this order: Pattern, Clarify, Brute force, Bottleneck, Optimised, Invariant, Time and space, Variation
- **And** each step stays hidden until I tap `I have it` for it

### 5. Each step is short

- **Given** a step is open
- **When** I read it
- **Then** each line is a short point, not a paragraph
- **And** the Brute force and Optimised steps show short pseudocode, the Optimised one in about 4 to 8 lines

### 6. A problem with no bottleneck

- **Given** the brute force of a problem does not repeat work, such as 155 Min Stack
- **When** I reveal the steps
- **Then** the card has no Bottleneck step
- **And** the step after Brute force is Optimised

### 7. A middle step that an interviewer can ask for

- **Given** a problem has a common solution between the brute force and the best one
- **When** I open the Optimised step
- **Then** I see one extra line that names that middle solution and its time

### 8. Answer the variation

- **Given** I opened the Variation step
- **When** I look at it
- **Then** I see one "what if" question and its answer is hidden
- **When** I tap `Show answer`
- **Then** I see the answer

### 9. Fill the gaps in the pseudocode

- **Given** I saw the cards of a pattern before
- **When** I open the Optimised step of a later card of that pattern
- **Then** some parts of the pseudocode are blank, such as the loop condition or the update step
- **When** I say the part in my head and tap the blank
- **Then** I see the hidden part

### 10. The first card of a pattern shows all of it

- **Given** I never saw a card of this pattern before
- **When** I open the Optimised step
- **Then** I see the full pseudocode with no blanks

### 11. Open the problem on LeetCode

- **Given** a card is on the screen
- **When** I tap the problem title
- **Then** the LeetCode page of the problem opens in a new tab

## Decisions

- **The pattern name is only on the back.** You chose on 20 Nov 2026 to drop the toggle on the front, because a peek gives the answer away.
- **The Pattern step comes first.** You chose this on 20 Nov 2026, over Clarify first and Pattern last.
- **Step by step, not one flip.** You chose this on 20 Nov 2026. A single flip makes it easy to skim.
- **No typing to commit.** You said on 20 Nov 2026 that you do not want to type on the commute, so you think each step and tap.
- **The longer card back stays.** You agreed on 20 Nov 2026 to the Bottleneck, Invariant and Variation steps.
- **The content can change later.** You said on 23 Nov 2026 that the card content is okay for now and the format is fine.
- **Look-alike patterns as the wrong choices.** You agreed to this on 26 Nov 2026. A random wrong choice is easy to rule out.
- **The labels `I have it` and `Show answer`.** You agreed to these on 26 Nov 2026.
