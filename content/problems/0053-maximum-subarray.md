---
number: 53
title: Maximum Subarray
difficulty: Medium
url: https://leetcode.com/problems/maximum-subarray/
pattern: greedy
template: running best
source: attempted
ask: Return the largest sum of a contiguous, non-empty subarray.
example: "[-2,1,-3,4,-1,2,1,-5,4] gives 6, from [4,-1,2,1]"
constraints: "1 <= n <= 10^5, -10^4 <= nums[i] <= 10^4"
---

## Pattern

- Greedy, running best template. Also known as Kadane's.
- Key: max(x, cur + x), start ans at -inf.

## Clarify

- Non-empty, so all negatives returns the biggest single number?
- Return the sum, not the subarray?

## Brute force

O(n²).

```
for i in range(n):
    s = 0
    for j in range(i, n):
        s += nums[j]
        best = max(best, s)
```

## Bottleneck

- Group subarrays by their end. Every subarray ending at j is one ending at j - 1, plus nums[j], or nums[j] alone.
- Adding nums[j] to all of them doesn't change which is biggest. So keep only the best ending at j - 1.

## Optimised

```
ans, cur = -inf, 0
for x in nums:
    cur = max(x, cur + x)
    ans = max(ans, cur)
return ans
```

## Invariant

- `cur` is the best sum of a subarray ending here. `ans` is the best anywhere so far.

## Time and space

- Time O(n).
- Space O(1).

## Variation

Q: What about the largest product (152)?

A: A negative times a negative flips small to big. Keep both the max and the min product ending here.
