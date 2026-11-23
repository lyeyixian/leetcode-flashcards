---
number: 300
title: Longest Increasing Subsequence
difficulty: Medium
url: https://leetcode.com/problems/longest-increasing-subsequence/
pattern: 1d-dp
template: look back at every earlier index
source: attempted
ask: Return the length of the longest strictly increasing subsequence.
example: "[10,9,2,5,3,7,101,18] gives 4, from 2, 3, 7, 101. [7,7,7,7] gives 1"
constraints: "1 <= n <= 2500, -10^4 <= nums[i] <= 10^4"
---

## Pattern

- 1-D DP, look back at every earlier index template.
- Key: longest ending at i, answer max(dp).

## Clarify

- Strictly increasing, so equal values don't extend?
- Subsequence, so you can skip items but keep order?

## Brute force

O(2^n). Every keep or skip choice.

```
def longest(i):         # ends at i
    return 1 + max((longest(j) for j < i if nums[j] < nums[i]), default=0)
return max(longest(i) for i in range(n))
```

## Bottleneck

- `longest(j)` is asked by every later i. Store it.

## Optimised

```
dp = [1] * n
for i in range(n):
    for j in range(i):
        if nums[j] < nums[i]: dp[i] = max(dp[i], dp[j] + 1)
return max(dp)
```

- Better: keep `tails`, where `tails[k]` is the smallest end of an increasing run of length k + 1. Binary search each number into it. O(n log n).

## Invariant

- dp[i] is the longest increasing subsequence that ends at nums[i]. "Ends at i" gives you the last item to compare.

## Time and space

- Time O(n²).
- Space O(n).

## Variation

Q: Why max(dp) and not dp[-1]?

A: The longest can end anywhere. On [1,3,5,2], dp is [1,2,3,2], and the answer is 3.
