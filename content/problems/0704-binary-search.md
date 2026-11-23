---
number: 704
title: Binary Search
difficulty: Easy
url: https://leetcode.com/problems/binary-search/
pattern: binary-search
template: exact match
source: attempted
ask: nums is sorted with distinct values. Return the index of target, or -1. Must be O(log n).
example: "nums = [-1,0,3,5,9,12], target = 9 gives 4"
constraints: "1 <= n <= 10^4, values distinct, sorted ascending"
---

## Pattern

- Binary search, exact match template.
- Key: drop the half on the wrong side of target.

## Clarify

- Sorted ascending, no duplicates?
- Return -1 when missing?

## Brute force

O(n).

```
for i, x in enumerate(nums):
    if x == target: return i
return -1
```

## Bottleneck

- Each check throws away one element. Sorted input lets each check throw away half.

## Optimised

```
lo, hi = 0, n - 1
while lo <= hi:
    mid = (lo + hi) // 2
    if nums[mid] == target: return mid
    if nums[mid] < target: lo = mid + 1
    else: hi = mid - 1
return -1
```

## Invariant

- If target is in nums, it is inside `[lo, hi]`.

## Time and space

- Time O(log n).
- Space O(1).

## Variation

Q: What if target can be missing and you need where it would go?

A: Use first true with `ok(mid) = nums[mid] >= target` and `hi = n`. That's 35 Search Insert Position.
