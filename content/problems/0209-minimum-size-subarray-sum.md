---
number: 209
title: Minimum Size Subarray Sum
difficulty: Medium
url: https://leetcode.com/problems/minimum-size-subarray-sum/
pattern: sliding-window
template: shortest window
source: attempted
ask: Return the length of the shortest contiguous subarray with sum >= target, or 0 if there is none. All numbers are positive.
example: "target = 7, nums = [2,3,1,2,4,3] gives 2, from [4,3]"
constraints: "1 <= target <= 10^9, 1 <= n <= 10^5, 1 <= nums[i] <= 10^4"
---

## Pattern

- Sliding window, shortest window template.
- Key: valid when sum >= target.

## Clarify

- All numbers positive? Yes, and that's what makes a window work.
- Return 0 when nothing reaches the target?
- n up to 10^5, so aim for O(n) or O(n log n).

## Brute force

O(n²).

```
for l in range(n):
    total = 0
    for r in range(l, n):
        total += nums[r]
        if total >= target:
            best = min(best, r - l + 1)
            break
```

## Bottleneck

- Each new start adds up the same numbers the last start already added.
- A running total for the window keeps that sum, so each number is added once and removed once.

## Optimised

```
l = 0, total = 0, best = inf
for r in range(n):
    total += nums[r]
    while total >= target:
        best = min(best, r - l + 1)
        total -= nums[l]; l += 1
return 0 if best == inf else best
```

- Middle step: prefix sums are sorted because nums are positive, so binary search the first end for each start. O(n log n).

## Invariant

- `total` is the sum of `nums[l..r]`.
- After the while, the window is below target, so every valid window ending at `r` was already recorded.

## Time and space

- Time O(n). `l` and `r` each move at most n times.
- Space O(1).

## Variation

Q: What if nums can be negative?

A: The window breaks, because adding a number can lower the sum. Use prefix sums with a monotonic deque, which is 862.
