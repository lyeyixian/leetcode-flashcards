---
number: 417
title: Pacific Atlantic Water Flow
difficulty: Medium
url: https://leetcode.com/problems/pacific-atlantic-water-flow/
pattern: graphs
template: search backwards
source: attempted
ask: Water flows to a neighbour of the same height or lower. Pacific is top and left, Atlantic is bottom and right. Return cells that reach both.
example: "[[1]] gives [[0,0]]"
constraints: "1 <= rows, cols <= 200, 0 <= heights[r][c] <= 10^5"
---

## Pattern

- Graphs, search backwards template.
- Key: uphill from each ocean's edges, answer is both sets.

## Clarify

- Same height counts as flowing?
- Edge cells flow straight into their ocean?
- Any order in the output?

## Brute force

O((r · c)²).

```
for each cell:
    DFS downhill, note if it reaches each ocean
    if both: add it
```

## Bottleneck

- Each cell's search re-walks paths its neighbours already walked.
- Flip it. Walk uphill from each ocean once. Each cell is visited once per ocean.

## Optimised

```
def dfs(r, c, seen):
    if (r, c) in seen: return
    seen.add((r, c))
    for each in-bounds neighbour (x, y):
        if heights[x][y] >= heights[r][c]: dfs(x, y, seen)
for r: dfs(r, 0, pac); dfs(r, C - 1, atl)
for c: dfs(0, c, pac); dfs(R - 1, c, atl)
return pac & atl
```

## Invariant

- `pac` holds exactly the cells whose water can reach the Pacific. Same for `atl`.

## Time and space

- Time O(r · c).
- Space O(r · c).

## Variation

Q: Why `>=` when water flows downhill?

A: You're walking the path backwards. The cell water came from has to be at least as high.
