---
number: 57
title: Insert Interval
difficulty: Medium
url: https://leetcode.com/problems/insert-interval/
pattern: intervals
template: insert in three parts
source: attempted
ask: intervals is sorted with no overlaps. Insert newInterval, merging where it overlaps. Touching counts.
example: "[[1,2],[3,5],[6,7],[8,10],[12,16]], new [4,8] gives [[1,2],[3,10],[12,16]]"
constraints: "0 <= n <= 10^4, sorted by start, no overlaps, 0 <= start <= end <= 10^5"
---

## Pattern

- Intervals, insert in three parts template.
- Key: before, overlapping, after.

## Clarify

- Already sorted and non-overlapping?
- Touching merges?
- Can intervals be empty?

## Brute force

O(n log n).

```
intervals.append(newInterval)
return merge(intervals)    # sort, then merge into the last
```

## Bottleneck

- The list is already sorted, so the sort is wasted.
- One pass in three parts does it.

## Optimised

```
ans, i = [], 0, ns, ne = newInterval
while i < n and intervals[i][1] < ns: ans.append(intervals[i]); i += 1
while i < n and intervals[i][0] <= ne:
    ns = min(ns, intervals[i][0]); ne = max(ne, intervals[i][1]); i += 1
ans.append([ns, ne])
ans.extend(intervals[i:])
return ans
```

## Invariant

- Part 1 stops at the first interval with `end >= ns`. So part 2 only needs `start <= ne` to know it overlaps.

## Time and space

- Time O(n).
- Space O(n) for the output.

## Variation

Q: Where does the new interval go if it's after everything, or the list is empty?

A: Both loops skip, and the append after them adds it. Putting the append inside a loop misses those cases.
