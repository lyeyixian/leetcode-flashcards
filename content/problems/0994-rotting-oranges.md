---
number: 994
title: Rotting Oranges
difficulty: Medium
url: https://leetcode.com/problems/rotting-oranges/
pattern: graphs
template: multi-source BFS
source: attempted
ask: 0 empty, 1 fresh, 2 rotten. Each minute, fresh oranges next to rotten ones rot. Return the minutes until none are fresh, or -1.
example: "[[2,1,1],[1,1,0],[0,1,1]] gives 4. [[0,2]] gives 0"
constraints: "1 <= rows, cols <= 10, cells are 0, 1 or 2"
---

## Pattern

- Graphs, multi-source BFS template.
- Key: all rotten in the queue at once, one level is one minute.

## Clarify

- Up, down, left, right only?
- No fresh orange at the start returns 0?
- An orange that can never rot returns -1?

## Brute force

O((r · c)²).

```
minutes = 0
loop:
    scan the grid, find fresh next to rotten
    if none: break
    rot them all; minutes += 1
return -1 if any fresh else minutes
```

## Bottleneck

- Each minute rescans cells that are already rotten or can't rot yet.
- Only the oranges that rotted last minute can spread. Keep them in a queue.

## Optimised

```
q = every rotten cell, fresh = count of 1s, minutes = 0
while q and fresh:
    for _ in range(len(q)):
        r, c = q.popleft()
        for each fresh neighbour (x, y):
            grid[x][y] = 2; fresh -= 1; q.append((x, y))
    minutes += 1
return -1 if fresh else minutes
```

## Invariant

- At the top of the while, `q` holds exactly the oranges that rotted at minute `minutes`.
- `while q and fresh` stops right after the last orange rots, so there's no extra minute.

## Time and space

- Time O(r · c).
- Space O(r · c).

## Variation

Q: What if you need each empty room's distance to the nearest gate (286)?

A: Same shape. Every gate starts in the queue, and each room gets the level it was reached at.
