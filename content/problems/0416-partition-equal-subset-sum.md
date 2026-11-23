---
number: 416
title: Partition Equal Subset Sum
difficulty: Medium
url: https://leetcode.com/problems/partition-equal-subset-sum/
pattern: 2d-dp
template: take or skip
source: attempted
ask: Return true if nums can be split into two groups with the same sum.
example: "[1,5,11,5] gives true. [1,2,3,5] gives false"
constraints: "1 <= n <= 200, 1 <= nums[i] <= 100"
---

## Pattern

- 2-D DP, take or skip template, each number once.
- Key: half the total, each number once, t goes down.

## Clarify

- Every number goes in exactly one group?
- All positive?
- Odd total is false straight away?

## Brute force

O(2^n).

```
def can(i, t):        # some of nums[:i] make t
    if t == 0: return True
    if i == 0 or t < 0: return False
    return can(i - 1, t) or can(i - 1, t - nums[i - 1])
return total % 2 == 0 and can(n, total // 2)
```

## Bottleneck

- Different take and skip orders reach the same (i, t). There are only n · target of them.
- t alone isn't enough. Which numbers are left depends on i.

## Optimised

```
if total % 2: return False
target = total // 2
dp = [False] * (target + 1); dp[0] = True
for x in nums:
    for t in range(target, x - 1, -1):
        dp[t] = dp[t] or dp[t - x]
return dp[target]
```

- Middle step: the full (n + 1) x (target + 1) table.

## Invariant

- After processing some numbers, dp[t] is True when some of them make t.
- t goes down so `dp[t - x]` still holds the row before x. Going up would use x twice.

## Time and space

- Time O(n · target).
- Space O(target).

## Variation

Q: What if you need the smallest possible difference between the two groups (1049)?

A: Same table. Find the biggest reachable t <= total // 2. The answer is `total - 2t`.
