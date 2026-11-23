---
number: 56
title: Merge Intervals
difficulty: Medium
url: https://leetcode.com/problems/merge-intervals/
pattern: intervals
template: merge into the last
source: attempted
ask: Merge every group of overlapping intervals. Touching counts as overlapping. The input isn't sorted.
example: "[[1,3],[2,6],[8,10],[15,18]] gives [[1,6],[8,10],[15,18]]. [[1,4],[4,5]] gives [[1,5]]"
constraints: "1 <= n <= 10^4, 0 <= start <= end <= 10^4"
---

## Pattern

- Intervals, merge into the last template.
- Key: sort by start, max on the end.

## Clarify

- Touching, like [1,4] and [4,5], merges?
- Is the input sorted? No.
- Any output order?

## Brute force

About O(n³).

```
repeat until nothing changes:
    for each pair i, j:
        if they overlap: replace both with their merge
```

## Bottleneck

- Most pairs are far apart and can't overlap.
- Sorted by start, an interval can only overlap the last merged one.

## Optimised

```
intervals.sort()
ans = []
for s, e in intervals:
    if ans and ans[-1][1] >= s: ans[-1][1] = max(ans[-1][1], e)
    else: ans.append([s, e])
return ans
```

## Invariant

- `ans` is merged and sorted, and nothing later starts before `ans[-1]` starts.
- `max` matters. [1,4] then [2,3] stays [1,4].

## Time and space

- Time O(n log n).
- Space O(n) for the sort and output.

## Variation

Q: What if the list is already sorted and you add one interval?

A: Skip the sort. Copy the ones before, merge the ones that overlap, copy the rest. That's 57 Insert Interval, O(n).
