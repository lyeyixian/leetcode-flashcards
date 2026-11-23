---
number: 153
title: Find Minimum in Rotated Sorted Array
difficulty: Medium
url: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/
pattern: binary-search
template: first true
source: attempted
ask: A sorted array of distinct values was rotated. Return the minimum in O(log n).
example: "[4,5,6,7,0,1,2] gives 0. [11,13,15,17] gives 11"
constraints: "1 <= n <= 5000, values distinct"
---

## Pattern

- Binary search, first true template.
- Key: first index with nums[mid] <= nums[-1].

## Clarify

- Distinct values? With duplicates it can be O(n).
- Could it be rotated n times, so it looks unrotated?

## Brute force

O(n).

```
return min(nums)
```

## Bottleneck

- The array isn't sorted, but `nums[i] <= nums[-1]` is false then true. It reads F F F T T T.
- The min is the first true.

## Optimised

```
lo, hi = 0, n - 1
while lo < hi:
    mid = (lo + hi) // 2
    if nums[mid] <= nums[-1]: hi = mid
    else: lo = mid + 1
return nums[lo]
```

## Invariant

- The min is inside `[lo, hi]`.
- `nums[mid] <= nums[-1]` means mid is in the same run as the last element, so the min is at mid or left of it.

## Time and space

- Time O(log n).
- Space O(1).

## Variation

Q: Why compare with nums[-1] and not nums[0]?

A: When the array isn't rotated, every element is >= nums[0], so the row is all true. Then "first false" points past the end.
