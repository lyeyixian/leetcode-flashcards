---
number: 503
title: Next Greater Element II
difficulty: Medium
url: https://leetcode.com/problems/next-greater-element-ii/
pattern: stack
template: resolve on pop
source: attempted
ask: nums is circular. For each element, return the first strictly greater value walking forward, wrapping around. -1 if none.
example: "[1,2,1] gives [2,-1,2]"
constraints: "1 <= n <= 10^4, -10^9 <= nums[i] <= 10^9"
---

## Pattern

- Monotonic stack, resolve on pop template.
- Key: second pass over nums, pop only.

## Clarify

- Negatives and duplicates allowed?
- Strictly greater, so an equal value doesn't count?
- Return the value, not the index or distance?

## Brute force

O(n²).

```
for i in range(n):
    for step in range(1, n):
        j = (i + step) % n
        if nums[j] > nums[i]:
            ans[i] = nums[j]; break
```

## Bottleneck

- Each element walks over the same later elements again.
- Keep the ones still waiting on a stack. Wrapping just means walking the array a second time.

## Optimised

```
ans = [-1] * n, stack = []
for i in range(2 * n):
    x = nums[i % n]
    while stack and nums[stack[-1]] < x:
        ans[stack.pop()] = x
    if i < n: stack.append(i)
return ans
```

## Invariant

- The stack holds indices with no greater value found yet.
- The second pass only answers them. Every element has been seen once already, so nothing new needs to wait.

## Time and space

- Time O(n). Two passes, each index pushed and popped at most once.
- Space O(n).

## Variation

Q: Which elements still hold -1 at the end?

A: Only copies of the maximum. Nothing anywhere beats them.
