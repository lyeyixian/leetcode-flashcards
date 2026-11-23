---
number: 875
title: Koko Eating Bananas
difficulty: Medium
url: https://leetcode.com/problems/koko-eating-bananas/
pattern: binary-search
template: search the answer
source: attempted
ask: Each hour Koko eats up to k bananas from one pile. Return the smallest k that finishes every pile within h hours.
example: "piles = [3,6,7,11], h = 8 gives 4"
constraints: "1 <= len(piles) <= 10^4, len(piles) <= h <= 10^9, 1 <= piles[i] <= 10^9"
---

## Pattern

- Binary search, search the answer template.
- Key: hours(k) <= h, k from 1 to max(piles).

## Clarify

- She eats from one pile per hour, and waits out the hour if it's done?
- h is at least the number of piles, so an answer always exists?
- piles up to 10^9, so trying every k is too slow.

## Brute force

O(n · m), m = max(piles).

```
for k in range(1, max(piles) + 1):
    if sum(ceil(p / k) for p in piles) <= h:
        return k
```

## Bottleneck

- The inner sum is needed. The outer loop steps k one at a time.
- A bigger k never takes more hours, so `hours(k) <= h` reads F F F T T T.

## Optimised

```
lo, hi = 1, max(piles)
while lo < hi:
    mid = (lo + hi) // 2
    hours = sum((p + mid - 1) // mid for p in piles)
    if hours <= h: hi = mid
    else: lo = mid + 1
return lo
```

## Invariant

- The answer is inside `[lo, hi]`. `max(piles)` always works, since it takes one hour per pile.

## Time and space

- Time O(n log m), m = max(piles).
- Space O(1).

## Variation

Q: Why not `lo = min(piles)`?

A: A k below the smallest pile can still work when h is large. Start at 1.
