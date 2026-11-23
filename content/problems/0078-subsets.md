---
number: 78
title: Subsets
difficulty: Medium
url: https://leetcode.com/problems/subsets/
pattern: backtracking
template: pick from the right
source: attempted
ask: nums holds distinct integers. Return every subset, the empty one included, in any order.
example: "[1,2,3] gives [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]"
constraints: "1 <= n <= 10, -10 <= nums[i] <= 10, all distinct"
---

## Pattern

- Backtracking, pick from the right template.
- Key: record at every call, recurse i + 1.

## Clarify

- All distinct?
- The empty subset counts?
- Any order of subsets?

## Brute force

O(n · 2^n). Every bitmask.

```
for mask in range(2 ** n):
    res.append([nums[i] for i in range(n) if mask >> i & 1])
```

## Bottleneck

- There are 2^n answers, so nothing beats O(n · 2^n). The bitmask is already optimal.
- Backtracking is the same cost, and it's the shape the harder versions build on.

## Optimised

```
res, path = [], []
def bt(start):
    res.append(path[:])
    for i in range(start, n):
        path.append(nums[i]); bt(i + 1); path.pop()
bt(0)
return res
```

## Invariant

- In `bt(start)`, path is a subset of `nums[:start]`, and only `nums[start:]` can be added.

## Time and space

- Time O(n · 2^n). 2^n subsets, each copied in O(n).
- Space O(n), not counting the output.

## Variation

Q: What if you only want subsets of size k?

A: Record only when `len(path) == k`, and return there. That's 77 Combinations.
