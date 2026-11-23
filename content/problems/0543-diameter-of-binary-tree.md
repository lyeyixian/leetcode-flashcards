---
number: 543
title: Diameter of Binary Tree
difficulty: Easy
url: https://leetcode.com/problems/diameter-of-binary-tree/
pattern: trees
template: bottom-up
source: attempted
ask: Return the number of edges on the longest path between any two nodes. It doesn't have to pass through the root.
example: "[1,2,3,4,5] gives 3, from 4 to 2 to 1 to 3"
constraints: "1 <= nodes <= 10^4, -100 <= val <= 100"
---

## Pattern

- Trees, bottom-up template, return one thing and record another.
- Key: return the height, record left + right.

## Clarify

- Edges, not nodes?
- The path may skip the root?

## Brute force

O(n²) on a chain.

```
for each node:
    best = max(best, height(node.left) + height(node.right))
```

## Bottleneck

- Each node measures its subtree heights from scratch, and those walks overlap.
- Height is already computed bottom-up. Record the diameter at the same time.

## Optimised

```
best = 0
def height(node):
    nonlocal best
    if node is None: return 0
    l, r = height(node.left), height(node.right)
    best = max(best, l + r)
    return 1 + max(l, r)
height(root)
return best
```

## Invariant

- `height(node)` returns the nodes on the longest path down. `l + r` is the edges on the longest path that turns at node.
- The parent can only extend one side, since a path can't fork. So return one side, record both.

## Time and space

- Time O(n).
- Space O(h).

## Variation

Q: What if each node has a value and you want the path with the biggest sum?

A: Same shape. Return `node.val + max(0, l, r)`, record `node.val + max(0, l) + max(0, r)`. That's 124 Binary Tree Maximum Path Sum.
