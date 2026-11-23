---
number: 435
title: Non-overlapping Intervals
difficulty: Medium
url: https://leetcode.com/problems/non-overlapping-intervals/
pattern: intervals
template: keep the earliest end
source: attempted
ask: Return the fewest intervals to remove so the rest don't overlap. Touching does not overlap here.
example: "[[1,2],[2,3],[3,4],[1,3]] gives 1. [[1,2],[1,2],[1,2]] gives 2"
constraints: "1 <= n <= 10^5, -5 * 10^4 <= start < end <= 5 * 10^4"
---

## Pattern

- Intervals, keep the earliest end template.
- Key: start >= prev end keeps it, answer n - kept.

## Clarify

- Touching, like [1,2] and [2,3], is fine?
- Fewest to remove is the same as most to keep?

## Brute force

O(2^n).

```
sort; def bt(i, prev_end):
    if i == n: return 0
    skip = bt(i + 1, prev_end)
    take = 1 + bt(i + 1, end_i) if start_i >= prev_end else 0
    return max(skip, take)
return n - bt(0, -inf)
```

- Middle step: `@cache` on `(i, prev_end)`. O(n²).

## Bottleneck

- You can prove one branch is never worse. Keeping the interval that ends first always leaves the most room.

## Optimised

```
intervals.sort(key=lambda x: x[1])
keep, prev = 0, -inf
for s, e in intervals:
    if s >= prev: keep += 1; prev = e
return n - keep
```

## Invariant

- Take a best answer sorted by end, x1, x2, ... Let y end earliest of all.
- y ends <= x1 ends <= x2 starts. Swap x1 for y: no new clash, same count. So some best answer starts with y.

## Time and space

- Time O(n log n).
- Space O(n) for the sort.

## Variation

Q: What if you want the fewest arrows that burst every balloon (452)?

A: Same loop. Touching balloons share an arrow, so it's `s > prev`, and the answer is the count, not n minus it.
