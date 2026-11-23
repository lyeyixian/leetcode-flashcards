---
number: 70
title: Climbing Stairs
difficulty: Easy
url: https://leetcode.com/problems/climbing-stairs/
pattern: 1d-dp
template: last two
source: attempted
ask: Each move climbs 1 or 2 steps. Return how many ways reach the top of n steps.
example: "n = 3 gives 3, from 1+1+1, 1+2, 2+1"
constraints: "1 <= n <= 45"
---

## Pattern

- 1-D DP, last two template.
- Key: dp[i - 1] + dp[i - 2], dp[0] = 1.

## Clarify

- Only 1 or 2 at a time?
- Order matters, so 1+2 and 2+1 are different?

## Brute force

O(2^n).

```
def ways(k):
    if k > n: return 0
    if k == n: return 1
    return ways(k + 1) + ways(k + 2)
```

## Bottleneck

- `ways(k)` is called many times and always returns the same number. It only depends on k.

## Optimised

```
prev = curr = 1
for _ in range(n - 1):
    prev, curr = curr, prev + curr
return curr
```

- Middle step: `@cache` on the brute force, or a dp array. O(n) space.

## Invariant

- dp[i] is the ways to reach step i. The last move onto i is a 1 or a 2, so dp[i] = dp[i - 1] + dp[i - 2].
- dp[0] = 1. Standing at the bottom is one way.

## Time and space

- Time O(n).
- Space O(1).

## Variation

Q: What if you can climb any step size in a list, like [1, 3, 5]?

A: dp[i] = sum of dp[i - s] for each size s <= i. That's the every choice from a list template.
