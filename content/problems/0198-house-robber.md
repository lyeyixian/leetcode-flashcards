---
number: 198
title: House Robber
difficulty: Medium
url: https://leetcode.com/problems/house-robber/
pattern: 1d-dp
template: last two
source: attempted
ask: You can't rob two houses next to each other. Return the most money you can rob.
example: "[2,7,9,3,1] gives 12. [2,1,1,2] gives 4, from houses 0 and 3"
constraints: "1 <= n <= 100, 0 <= nums[i] <= 400"
---

## Pattern

- 1-D DP, last two template.
- Key: max(skip, rob + two back).

## Clarify

- Values all non-negative?
- Skipping two in a row is allowed?

## Brute force

O(2^n).

```
def best(i):
    if i >= n: return 0
    return max(nums[i] + best(i + 2), best(i + 1))
```

## Bottleneck

- `best(i)` only depends on i, and is asked again and again.
- Greedy fails. Every other house misses [2,1,1,2], and biggest first misses [2,7,9,8,1].

## Optimised

```
prev = curr = 0
for x in nums:
    prev, curr = curr, max(curr, prev + x)
return curr
```

## Invariant

- `curr` is the most from the houses so far. `prev` is the most from all but the last one.

## Time and space

- Time O(n).
- Space O(1).

## Variation

Q: What if the houses are in a circle (213)?

A: The first and last can't both be robbed. Run this twice, once without the first house and once without the last, and take the max.
