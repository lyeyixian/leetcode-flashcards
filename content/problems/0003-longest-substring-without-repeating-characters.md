---
number: 3
title: Longest Substring Without Repeating Characters
difficulty: Medium
url: https://leetcode.com/problems/longest-substring-without-repeating-characters/
pattern: sliding-window
template: longest window
source: attempted
ask: Return the length of the longest substring of s with no repeated character.
example: "\"pwwkew\" gives 3, from \"wke\". \"pwke\" is a subsequence, not a substring"
constraints: "0 <= len(s) <= 5 * 10^4, letters, digits, symbols and spaces"
---

## Pattern

- Sliding window, longest window template.
- Key: valid when s[r] isn't already in the window.

## Clarify

- Substring means contiguous, not a subsequence?
- Any ASCII, including spaces? Is the empty string allowed? It returns 0.
- n up to 5 * 10^4, so aim for O(n).

## Brute force

O(n³), or O(n²) with a set per start.

```
for l in range(n):
    for r in range(l, n):
        if s[l..r] has no repeats:
            best = max(best, r - l + 1)
```

## Bottleneck

- Each new start rechecks letters the last start already checked.
- A set of the window's letters keeps that, so each letter goes in and out once.

## Optimised

```
l = 0, best = 0, seen = set()
for r, c in enumerate(s):
    while c in seen:
        seen.remove(s[l]); l += 1
    seen.add(c)
    best = max(best, r - l + 1)
return best
```

## Invariant

- After adding `c`, the window holds no repeated character, and `seen` is exactly its letters.

## Time and space

- Time O(n). Each letter is added and removed at most once.
- Space O(min(n, alphabet)).

## Variation

Q: Can `l` jump instead of stepping one at a time?

A: Yes. Keep the last index of each letter in a dict and set `l = max(l, last[c] + 1)`. The `max` stops `l` moving back to a repeat that is already outside the window.
