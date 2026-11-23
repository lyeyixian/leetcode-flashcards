---
number: 11
title: Container With Most Water
difficulty: Medium
url: https://leetcode.com/problems/container-with-most-water/
pattern: two-pointers
template: opposite ends
source: attempted
ask: height[i] is a line at x = i. Pick two lines that hold the most water with the x-axis. Return that area.
example: "[1,8,6,2,5,4,8,3,7] gives 49, from the lines at 1 and 8"
constraints: "2 <= n <= 10^5, 0 <= height[i] <= 10^4"
---

## Pattern

- Two pointers, opposite ends template.
- Key: move the shorter line.

## Clarify

- Area is (j - i) * min(height[i], height[j])?
- Lines have no width, and you can't tilt the container?
- n up to 10^5, so O(n²) is too slow.

## Brute force

O(n²).

```
for i in range(n):
    for j in range(i + 1, n):
        best = max(best, (j - i) * min(height[i], height[j]))
```

## Bottleneck

- Most pairs can't win. The area is capped by the shorter line.
- Moving inward always shrinks the width, so only a taller line can help.

## Optimised

```
l, r = 0, n - 1, best = 0
while l < r:
    best = max(best, (r - l) * min(height[l], height[r]))
    if height[l] < height[r]: l += 1
    else: r -= 1
return best
```

## Invariant

- Start at the widest pair.
- Every other pair with the shorter line is narrower and no taller, so it can't beat what you just recorded. Drop it.

## Time and space

- Time O(n).
- Space O(1).

## Variation

Q: What if both lines are the same height?

A: Move either one. Any pair that keeps one of them is narrower and capped at the same height, so neither can do better.
