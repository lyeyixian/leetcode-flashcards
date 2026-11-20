---
number: 567
title: Permutation in String
difficulty: Medium
url: https://leetcode.com/problems/permutation-in-string/
pattern: sliding-window
template: fixed size window
source: attempted
ask: Return true if some rearrangement of s1 appears as a substring of s2.
example: "s1 = \"ab\", s2 = \"eidbaooo\" gives true, from \"ba\""
constraints: "1 <= len(s1), len(s2) <= 10^4, lowercase a to z"
---

## Pattern

- Sliding window, fixed size window template, size len(s1).
- Key: match when every count is 0.

## Clarify

- Lowercase a to z only?
- What if s1 is longer than s2? Return false straight away.
- n up to 10^4, so O(n · 26) is fine.

## Brute force

O(n · m), n = len(s2), m = len(s1).

```
for l in range(n - m + 1):
    if count(s2[l : l + m]) == count(s1):
        return True
return False
```

## Bottleneck

- Each window recounts m - 1 letters the last window already counted.
- Sliding by one changes only two counts, the letter coming in and the letter going out.

## Optimised

```
if m > n: return False
diff = count(s1)
for r, c in enumerate(s2):
    diff[c] -= 1
    if r >= m:
        diff[s2[r - m]] += 1
    if r >= m - 1 and all counts in diff are 0:
        return True
return False
```

## Invariant

- `diff` holds s1's counts minus the window's counts. All zero means the window is a rearrangement of s1.

## Time and space

- Time O(26n), so O(n). The all-zero check looks at 26 counts.
- Space O(26).

## Variation

Q: What if you need every start index of a match, not just whether one exists?

A: Same loop, but append `r - m + 1` instead of returning. That's 438 Find All Anagrams in a String.
