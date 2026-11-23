---
number: 133
title: Clone Graph
difficulty: Medium
url: https://leetcode.com/problems/clone-graph/
pattern: graphs
template: DFS with a dict
source: attempted
ask: Given one node of a connected undirected graph, return a deep copy of the whole graph.
example: "adj = [[2,4],[1,3],[2,4],[1,3]] gives a copy of the square 1-2-3-4-1"
constraints: "0 <= nodes <= 100, vals 1 to n and unique, no repeated edges, no self-loops"
---

## Pattern

- Graphs, DFS with a dict template.
- Key: old to copy, store before the neighbour loop.

## Clarify

- Can the input be None?
- Connected, so one start reaches everything?
- Vals unique? You still key on the node, not the val.

## Brute force

- Copy with no memory: 1 copies 2, 2 copies 1, forever.
- A visited set stops that, but when 2 reaches 1 again there's no copy of 1 to link to.

## Optimised

```
copies = {}
def dfs(old):
    if old in copies: return copies[old]
    new = Node(old.val)
    copies[old] = new
    for nb in old.neighbors: new.neighbors.append(dfs(nb))
    return new
return dfs(node) if node else None
```

## Invariant

- `copies` holds every node you've started copying. It's the visited set and the lookup at once.
- Storing before the loop means a cycle back to `old` finds its copy.

## Time and space

- Time O(V + E).
- Space O(V) for the dict and call stack, not counting the copy.

## Variation

Q: What if it's a linked list with a random pointer (138)?

A: Same dict from old node to copy. Copy `next` and `random` through it.
