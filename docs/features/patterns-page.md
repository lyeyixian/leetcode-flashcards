# Patterns page

The patterns page lists all the patterns. Each pattern has its own page with how to recognise it, its templates and its problems. You use it to study a pattern before or after you practise its cards.

**As** the person who practises LeetCode on the commute,
**I want** to read each pattern, its templates and how to spot it,
**so that** I can match a new problem to the right template.

## Acceptance criteria

### 1. See the list of patterns

- **Given** I am on any page
- **When** I open `Patterns`
- **Then** I see the 17 patterns in the order of the study plan, starting with Arrays and hashing
- **And** each pattern shows the number of its problems

### 2. Open a pattern

- **Given** I am on `Patterns`
- **When** I tap a pattern
- **Then** I see its page with these sections, in this order: Core idea, Recognise it, Brute force it beats, Templates, Problems, Cost

### 3. Recognise a pattern

- **Given** I am on a pattern page
- **When** I read Recognise it
- **Then** I see the cue words in a problem, the questions to ask, and the problems that look like this pattern but need another one

### 4. Read a template

- **Given** I am on a pattern page
- **When** I read a template
- **Then** I see its name, its cue, its order in words, and its code
- **And** the name says what it does, such as `longest window`, not a letter such as A or B

### 5. Go from a problem to its card

- **Given** I am on a pattern page
- **When** I look at Problems
- **Then** I see each problem with its number, title, template and one-line key
- **When** I tap a problem
- **Then** I see its card, with all the steps open

### 6. A pattern with no problems yet

- **Given** a pattern has no problem that I attempted
- **When** I open its page
- **Then** I see the sections that have content
- **And** Problems shows `No problems yet`

## Assumptions

- The 17 patterns follow your practice folder order. Sliding window is number 3.
- Bit manipulation has almost no notes today, so its page is short.
- The label `No problems yet` is my proposal.

## Decisions

- **Recognise it comes before the templates.** You agreed to this on 20 Nov 2026. The goal is to name the pattern in 2 minutes.
- **Template names, not letters.** You said on 20 Nov 2026 that you will not remember A, B or C and will not say them to an interviewer.
- **Each template keeps its order in words above the code.** You agreed to this on 20 Nov 2026, because many past misses were order mistakes.
