---
number: 62
title: Unique Paths
difficulty: Medium
url: https://leetcode.com/problems/unique-paths/
pattern: 2d-dp
template: grid paths
source: attempted
ask: A robot moves only right or down on an m x n grid. Return how many paths go from top-left to bottom-right.
example: "m = 3, n = 7 gives 28. m = 3, n = 2 gives 3"
constraints: "1 <= m, n <= 100, the answer is at most 2 * 10^9"
---

## Pattern

- 2-D DP, grid paths template.
- Key: up + left, top row and left column are 1.

## Clarify

- Only right and down?
- 1 x 1 gives 1?

## Brute force

O(2^(m + n)).

```
def paths(r, c):
    if r < 0 or c < 0: return 0
    if r == 0 and c == 0: return 1
    return paths(r - 1, c) + paths(r, c - 1)
```

## Bottleneck

- Paths to (r, c) only depend on (r, c), and the same cells are asked from many routes.

## Optimised

```
dp = [[1] * n for _ in range(m)]
for r in range(1, m):
    for c in range(1, n):
        dp[r][c] = dp[r - 1][c] + dp[r][c - 1]
return dp[m - 1][n - 1]
```

- Better: one row, `row[c] += row[c - 1]`. O(n) space.

## Invariant

- dp[r][c] is the number of paths to (r, c). The last move is from above or from the left, and those don't overlap.

## Time and space

- Time O(m · n).
- Space O(m · n), or O(n) with one row.

## Variation

Q: What if some cells are blocked (63)?

A: A blocked cell is 0. The top row and left column are 1 only up to the first block.
