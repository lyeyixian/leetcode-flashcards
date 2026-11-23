---
number: 39
title: Combination Sum
difficulty: Medium
url: https://leetcode.com/problems/combination-sum/
pattern: backtracking
template: running total
source: attempted
ask: candidates are distinct positive integers. Return every combination that adds up to target. Each number can be used many times.
example: "candidates = [2,3,6,7], target = 7 gives [[2,2,3],[7]]"
constraints: "1 <= len(candidates) <= 30, 2 <= candidates[i] <= 40, distinct, 1 <= target <= 40"
---

## Pattern

- Backtracking, running total template.
- Key: recurse with i, break when too big.

## Clarify

- Same number many times?
- [2,2,3] and [3,2,2] count once?
- All positive, so the total only grows?

## Brute force

- Try every sequence of picks until the sum reaches or passes target. Order matters, so the same set shows up many times.

```
def bt(total):
    if total == target: add sorted(path) to a set
    if total >= target: return
    for x in candidates: path.append(x); bt(total + x); path.pop()
```

## Bottleneck

- Going back left rebuilds the same set in a new order.
- Never go left, and stop as soon as a candidate is too big.

## Optimised

```
candidates.sort()
def bt(start, remaining):
    if remaining == 0: res.append(path[:]); return
    for i in range(start, len(candidates)):
        if candidates[i] > remaining: break
        path.append(candidates[i])
        bt(i, remaining - candidates[i])
        path.pop()
bt(0, target)
```

## Invariant

- path adds up to `target - remaining`, and is never decreasing.

## Time and space

- Time O(n^(T/m)) as a loose bound, T = target, m = the smallest candidate.
- Space O(T/m), the deepest path.

## Variation

Q: What if each candidate can be used once and candidates can repeat (40)?

A: Recurse with `i + 1`, and skip `i > start` repeats after sorting.
