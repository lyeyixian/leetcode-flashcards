---
number: 55
title: Jump Game
difficulty: Medium
url: https://leetcode.com/problems/jump-game/
pattern: greedy
template: farthest reach
source: attempted
ask: From index i you can jump to anything up to i + nums[i]. Starting at 0, return true if you can reach the last index.
example: "[2,3,1,1,4] gives true. [3,2,1,0,4] gives false"
constraints: "1 <= n <= 10^4, 0 <= nums[i] <= 10^5"
---

## Pattern

- Greedy, farthest reach template.
- Key: fail when i > far.

## Clarify

- nums[i] is the longest jump, and shorter ones are allowed?
- One element means you're already there?

## Brute force

Exponential.

```
def can(i):
    if i >= n - 1: return True
    return any(can(i + j) for j in range(1, nums[i] + 1))
```

## Bottleneck

- Many paths land on the same index and redo the same search.
- Shorter jumps are allowed, so the reachable spots are a solid block. One number holds it.

## Optimised

```
far = 0
for i, x in enumerate(nums):
    if i > far: return False
    far = max(far, i + x)
return True
```

## Invariant

- Every index from 0 to `far` is reachable, and nothing past it is yet.

## Time and space

- Time O(n).
- Space O(1).

## Variation

Q: What if jumps have to be exactly nums[i] (in either direction, 1306)?

A: The reachable set has holes, so one number isn't enough. BFS with a visited set.
