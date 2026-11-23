---
number: 287
title: Find the Duplicate Number
difficulty: Medium
url: https://leetcode.com/problems/find-the-duplicate-number/
pattern: linked-list
template: index as a hash key
source: attempted
ask: nums has n + 1 values in 1..n, and one value repeats. Return it without changing nums, in O(1) extra space.
example: "[1,3,4,2,2] gives 2. [3,3,3,3,3] gives 3"
constraints: "1 <= n <= 10^5, len(nums) = n + 1, 1 <= nums[i] <= n"
---

## Pattern

- Index as a hash key, followed as a linked list, then fast and slow.
- Key: follow i -> nums[i], the cycle start is the repeat.

## Clarify

- Exactly one value repeats, maybe more than twice?
- Can't change nums, and O(1) space?

## Brute force

O(n) time, O(n) space. Or sort, which changes nums.

```
seen = set()
for x in nums:
    if x in seen: return x
    seen.add(x)
```

## Bottleneck

- Marking would change nums, and a set costs O(n).
- Read `i -> nums[i]` as a next pointer. The repeated value has two arrows into it, so it's where the cycle starts.

## Optimised

```
slow = fast = 0
while True:
    slow, fast = nums[slow], nums[nums[fast]]
    if slow == fast: break
slow = 0
while slow != fast:
    slow, fast = nums[slow], nums[fast]
return slow
```

## Invariant

- Index 0 has no arrows in, since no value is 0. So the walk from 0 has a tail before the cycle, like 142.

## Time and space

- Time O(n).
- Space O(1).

## Variation

Q: Is there another O(1) space way?

A: Binary search on the value. Count how many are `<= mid`. More than mid means the repeat is `<= mid`. O(n log n).
