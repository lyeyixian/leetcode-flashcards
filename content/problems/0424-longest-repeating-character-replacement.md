---
number: 424
title: Longest Repeating Character Replacement
difficulty: Medium
url: https://leetcode.com/problems/longest-repeating-character-replacement/
pattern: sliding-window
template: longest window
source: attempted
ask: Change at most k letters in s. Return the length of the longest substring of one repeated letter.
example: "s = \"AABABBA\", k = 1 gives 4, from \"AABA\" to \"AAAA\""
constraints: "1 <= len(s) <= 10^5, uppercase A to Z, 0 <= k <= len(s)"
---

## Pattern

- Sliding window, longest window template.
- Key: valid when window length - top count <= k.

## Clarify

- Uppercase A to Z only?
- Can k be 0? Can k be as big as len(s)?
- n up to 10^5, so aim for O(n) or O(n log n).

## Brute force

O(n²) with running counts per start.

```
for l in range(n):
    count = {}
    for r in range(l, n):
        count[s[r]] += 1
        if (r - l + 1) - max(count) <= k:
            best = max(best, r - l + 1)
```

## Bottleneck

- Each new start recounts letters the last start already counted.
- A window keeps the counts, so each letter goes in and out once.

## Optimised

```
l = 0, best = 0, count = {}
for r in range(n):
    count[s[r]] += 1
    while (r - l + 1) - max(count) > k:
        count[s[l]] -= 1; l += 1
    best = max(best, r - l + 1)
return best
```

## Invariant

- After the shrink, window length - top count <= k.
- Letters to change = window length - top count. No need to add up the other counts.

## Time and space

- Time O(26n), so O(n). `max` runs over 26 counts each step.
- Space O(26).

## Variation

Q: What if you must return the substring itself?

A: Keep the `l` of the best window as well as its length, and slice once at the end.
