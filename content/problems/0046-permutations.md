---
number: 46
title: Permutations
difficulty: Medium
url: https://leetcode.com/problems/permutations/
pattern: backtracking
template: any unused
source: attempted
ask: nums holds distinct integers. Return every ordering of all of them.
example: "[1,2,3] gives [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]"
constraints: "1 <= n <= 6, -10 <= nums[i] <= 10, all distinct"
---

## Pattern

- Backtracking, any unused template.
- Key: used flags, record when full.

## Clarify

- All distinct?
- [1,2] and [2,1] are different answers?

## Brute force

- n nested loops, one per position, each skipping numbers already taken. You can't write them when n varies.

```
for a in nums:
    for b in nums if b != a:
        for c in nums if c not in (a, b):
            res.append([a, b, c])
```

## Bottleneck

- There are n! answers, so nothing beats O(n · n!). Recursion just writes the n loops for you.

## Optimised

```
used = [False] * n
def bt():
    if len(path) == n: res.append(path[:]); return
    for i in range(n):
        if used[i]: continue
        used[i] = True; path.append(nums[i])
        bt()
        path.pop(); used[i] = False
```

## Invariant

- `used[i]` is True exactly while `nums[i]` is in path.

## Time and space

- Time O(n · n!).
- Space O(n).

## Variation

Q: What if nums can have repeats (47)?

A: Sort. Skip `i` when `nums[i] == nums[i - 1]` and `used[i - 1]` is False. That stops two equal values swapping places.
