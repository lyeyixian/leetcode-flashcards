---
number: 167
title: Two Sum II - Input Array Is Sorted
difficulty: Medium
url: https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/
pattern: two-pointers
template: opposite ends
source: attempted
ask: numbers is sorted and 1-indexed. Return the 1-indexed positions of the two numbers that add up to target. Use O(1) extra space.
example: "numbers = [2,7,11,15], target = 9 gives [1,2]"
constraints: "2 <= n <= 3 * 10^4, -1000 <= numbers[i] <= 1000, exactly one answer"
---

## Pattern

- Two pointers, opposite ends template.
- Key: sum too small, move left. Too big, move right.

## Clarify

- Exactly one answer? Can I use the same element twice? No.
- 1-indexed output?
- O(1) extra space, so no hash map.

## Brute force

O(n²).

```
for i in range(n):
    for j in range(i + 1, n):
        if numbers[i] + numbers[j] == target:
            return [i + 1, j + 1]
```

## Bottleneck

- The inner loop tries every j, but the array is sorted.
- One sum tells you whether every pair with `left` is too small, or every pair with `right` is too big.

## Optimised

```
l, r = 0, n - 1
while l < r:
    s = numbers[l] + numbers[r]
    if s == target: return [l + 1, r + 1]
    if s < target: l += 1
    else: r -= 1
```

## Invariant

- The answer is always inside `[l, r]`.
- Sum too small: `l` pairs with nothing bigger than `numbers[r]`, so all its pairs are too small.
- Sum too big: same for `r` the other way.

## Time and space

- Time O(n). Each step moves one pointer.
- Space O(1).

## Variation

Q: What if the array is not sorted and you need the original indices?

A: Use a hash map of value to index, which is Two Sum (1). Sorting would lose the indices.
