---
number: 452
title: Minimum Number of Arrows to Burst Balloons
difficulty: Medium
url: https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/
pattern: intervals
template: keep the earliest end
source: attempted
ask: Each balloon is [x_start, x_end]. An arrow at x bursts every balloon with start <= x <= end. Return the fewest arrows.
example: "[[10,16],[2,8],[1,6],[7,12]] gives 2. [[1,2],[2,3],[3,4],[4,5]] gives 2"
constraints: "1 <= n <= 10^5, -2^31 <= x_start < x_end <= 2^31 - 1"
---

## Pattern

- Intervals, keep the earliest end template.
- Key: start > prev end needs a new arrow.

## Clarify

- An arrow at 2 bursts both [1,2] and [2,3]?
- Values go down to -2^31, so start the last arrow at -inf?

## Brute force

- Try every set of arrow spots at balloon ends, smallest sets first. Exponential.

## Bottleneck

- Some arrow must hit the balloon that ends first, at or before its end.
- Sliding it right to that end still hits everything it hit. So shoot at the earliest end, every time.

## Optimised

```
points.sort(key=lambda p: p[1])
arrows, last = 0, -inf
for s, e in points:
    if s > last: arrows += 1; last = e
return arrows
```

## Invariant

- `last` is where the latest arrow went. Every balloon so far that starts at or before `last` is already burst.

## Time and space

- Time O(n log n).
- Space O(n) for the sort.

## Variation

Q: What breaks if `last` starts at 0 or -2^31?

A: A balloon like [-2^31, 0] looks already burst, so it never gets an arrow.
