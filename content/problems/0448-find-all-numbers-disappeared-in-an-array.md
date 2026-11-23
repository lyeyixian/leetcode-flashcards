---
number: 448
title: Find All Numbers Disappeared in an Array
difficulty: Easy
url: https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/
pattern: linked-list
template: index as a hash key
source: attempted
ask: nums has n values, each in 1..n. Return every value in 1..n that's missing. O(n) time, no extra space apart from the output.
example: "[4,3,2,7,8,2,3,1] gives [5,6]"
constraints: "1 <= n <= 10^5, 1 <= nums[i] <= n"
---

## Pattern

- Index as a hash key template.
- Key: negate nums[v - 1], positives are missing.

## Clarify

- Every value is in 1..n?
- Can I change the input?
- Does the output count as extra space? No.

## Brute force

O(n) time, O(n) space.

```
seen = [False] * (n + 1)
for x in nums: seen[x] = True
return [v for v in 1..n if not seen[v]]
```

## Bottleneck

- The seen array costs O(n). Every value names a slot in nums itself, so mark there.

## Optimised

```
for x in nums:
    i = abs(x) - 1
    nums[i] = -abs(nums[i])
return [i + 1 for i, x in enumerate(nums) if x > 0]
```

## Invariant

- `nums[i] < 0` means value i + 1 was seen. The size of each value is unchanged, so `abs` still reads it.

## Time and space

- Time O(n).
- Space O(1), not counting the output.

## Variation

Q: What if you need the values that appear twice instead?

A: Same marking. If `nums[i]` is already negative when you reach it, `abs(x)` is a repeat. That's 442.
