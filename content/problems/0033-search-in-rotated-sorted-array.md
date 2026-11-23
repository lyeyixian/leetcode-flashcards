---
number: 33
title: Search in Rotated Sorted Array
difficulty: Medium
url: https://leetcode.com/problems/search-in-rotated-sorted-array/
pattern: binary-search
template: exact match
source: attempted
ask: nums is a rotated sorted array of distinct values. Return the index of target, or -1, in O(log n).
example: "nums = [4,5,6,7,0,1,2], target = 0 gives 4. target = 3 gives -1"
constraints: "1 <= n <= 5000, values distinct"
---

## Pattern

- Binary search, exact match template, with one extra question at each mid.
- Key: one half is sorted, is target in its range?

## Clarify

- Distinct values?
- Return -1 when missing?

## Brute force

O(n).

```
for i, x in enumerate(nums):
    if x == target: return i
return -1
```

## Bottleneck

- The whole array isn't sorted, but one half around mid always is.
- A sorted half tells you in O(1) whether target can be in it.

## Optimised

```
lo, hi = 0, n - 1
while lo <= hi:
    mid = (lo + hi) // 2
    if nums[mid] == target: return mid
    if nums[lo] <= nums[mid]:                  # left half sorted
        if nums[lo] <= target < nums[mid]: hi = mid - 1
        else: lo = mid + 1
    else:                                      # right half sorted
        if nums[mid] < target <= nums[hi]: lo = mid + 1
        else: hi = mid - 1
return -1
```

- Other way: find the min with 153, then run plain binary search on the side that holds target.

## Invariant

- If target is in nums, it is inside `[lo, hi]`.
- If target isn't inside the sorted half's range, it can't be in that half.

## Time and space

- Time O(log n).
- Space O(1).

## Variation

Q: What if values can repeat (81)?

A: When `nums[lo] == nums[mid] == nums[hi]`, you can't tell which half is sorted. Move `lo += 1` and `hi -= 1`. The worst case becomes O(n).
