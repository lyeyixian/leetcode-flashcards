---
number: 238
title: Product of Array Except Self
difficulty: Medium
url: https://leetcode.com/problems/product-of-array-except-self/
pattern: arrays-and-hashing
template: prefix and suffix
source: attempted
ask: Return answer where answer[i] is the product of every element except nums[i]. O(n), no division.
example: "[1,2,3,4] gives [24,12,8,6]"
constraints: "2 <= n <= 10^5, -30 <= nums[i] <= 30, the product fits in 32 bits"
---

## Pattern

- Arrays and hashing, prefix and suffix template.
- Key: left product times right product.

## Clarify

- No division allowed? Division also breaks on zeros.
- Does the output array count as extra space? Usually not.

## Brute force

O(n²).

```
for i in range(n):
    p = 1
    for j in range(n):
        if j != i: p *= nums[j]
    ans[i] = p
```

## Bottleneck

- Each i multiplies almost the same numbers again.
- answer[i] = product left of i times product right of i. Both can be built in one pass each.

## Optimised

```
ans = [1] * n, p = 1
for i in range(n):
    ans[i] = p; p *= nums[i]
p = 1
for i in range(n - 1, -1, -1):
    ans[i] *= p; p *= nums[i]
return ans
```

- Middle step: separate `prefix` and `suffix` arrays, then multiply. O(n) space.

## Invariant

- Before the update at i, `p` is the product of everything on one side of i, not counting i.

## Time and space

- Time O(n), two passes.
- Space O(1), not counting the output.

## Variation

Q: What if division were allowed?

A: Count the zeros. Two or more, every answer is 0. One, only the zero's slot gets the product of the rest. None, divide the total by nums[i].
