---
number: 128
title: Longest Consecutive Sequence
difficulty: Medium
url: https://leetcode.com/problems/longest-consecutive-sequence/
pattern: arrays-and-hashing
template: set with run start
source: attempted
ask: Return the length of the longest run of consecutive integers in nums, in any order. Must be O(n).
example: "[100,4,200,1,3,2] gives 4, from 1, 2, 3, 4"
constraints: "0 <= n <= 10^5, -10^9 <= nums[i] <= 10^9"
---

## Pattern

- Arrays and hashing, set with run start template.
- Key: only walk up from x when x - 1 is missing.

## Clarify

- Duplicates count once?
- Empty input returns 0?
- O(n) is required, so no sorting.

## Brute force

O(n log n) with a sort. O(n³) with no sort and a scan for each next number.

```
nums.sort()
for i in range(1, n):
    if nums[i] == nums[i - 1]: continue
    if nums[i] == nums[i - 1] + 1: cur += 1
    else: cur = 1
    best = max(best, cur)
```

## Bottleneck

- Sorting is only there to answer "is x + 1 present?". A set answers that in O(1).
- Walking up from every number repeats work. Start only from numbers that begin a run.

## Optimised

```
s = set(nums), best = 0
for x in s:
    if x - 1 in s: continue
    end = x
    while end + 1 in s: end += 1
    best = max(best, end - x + 1)
return best
```

## Invariant

- The while loop only runs from the start of a run, so each number is walked over once in total.

## Time and space

- Time O(n). The inner loop runs at most n times across the whole outer loop.
- Space O(n) for the set.

## Variation

Q: Why loop over the set and not over nums?

A: With many duplicates of a run's start, looping nums walks that run once per copy. That can be O(n²).
