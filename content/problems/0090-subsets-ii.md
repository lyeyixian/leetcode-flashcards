---
number: 90
title: Subsets II
difficulty: Medium
url: https://leetcode.com/problems/subsets-ii/
pattern: backtracking
template: repeats in the input
source: attempted
ask: nums may contain repeats. Return every different subset once.
example: "[1,2,2] gives [[],[1],[1,2],[1,2,2],[2],[2,2]]"
constraints: "1 <= n <= 10, -10 <= nums[i] <= 10"
---

## Pattern

- Backtracking, repeats in the input template.
- Key: sort, skip i > start repeats.

## Clarify

- [1,2] from the first 2 and from the second 2 count once?
- Any order?

## Brute force

O(n · 2^n), plus a set.

```
seen = set()
for every subset s of nums:
    seen.add(tuple(sorted(s)))
return [list(t) for t in seen]
```

## Bottleneck

- The set still walks every repeated branch, then throws copies away.
- Sort, and don't start the same branch twice from one loop.

## Optimised

```
nums.sort()
def bt(start):
    res.append(path[:])
    for i in range(start, n):
        if i > start and nums[i] == nums[i - 1]: continue
        path.append(nums[i]); bt(i + 1); path.pop()
bt(0)
```

## Invariant

- In one loop, each value is the first pick at most once.
- `i > start`, not `i > 0`. A repeat one level deeper is a new slot, which is how [1,2,2] is built.

## Time and space

- Time O(n · 2^n).
- Space O(n).

## Variation

Q: What breaks with `i > 0` instead of `i > start`?

A: It also skips the second 2 as the first pick one level down, so [1,2,2] and [2,2] are lost.
