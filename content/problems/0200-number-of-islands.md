---
number: 200
title: Number of Islands
difficulty: Medium
url: https://leetcode.com/problems/number-of-islands/
pattern: graphs
template: flood fill
source: attempted
ask: grid has "1" for land and "0" for water. Land joins up, down, left and right. Return how many islands there are.
example: "[[1,1,0,0,0],[1,1,0,0,0],[0,0,1,0,0],[0,0,0,1,1]] gives 3"
constraints: "1 <= rows, cols <= 300, cells are \"0\" or \"1\""
---

## Pattern

- Graphs, flood fill template.
- Key: count each unseen land cell, then sink its island.

## Clarify

- No diagonals?
- Outside the grid is water?
- Can I change the grid? If not, use a seen set.

## Brute force

- A flood with no marking visits cells forever. Some memory of what's done is the whole answer.

## Optimised

```
def dfs(r, c):
    if out of bounds or grid[r][c] != "1": return
    grid[r][c] = "0"
    dfs four neighbours
count = 0
for every cell:
    if grid[r][c] == "1": count += 1; dfs(r, c)
return count
```

## Invariant

- After a cell's flood, every cell of its island is water. The outer loop can't count it again.

## Time and space

- Time O(r · c). Each cell turns to water once, even with the DFS inside two loops.
- Space O(r · c) for the call stack on a grid of all land.

## Variation

Q: What if you need the size of the biggest island?

A: Have `dfs` return 1 plus the four calls, and keep the max. That's 695 Max Area of Island.
