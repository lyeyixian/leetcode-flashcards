---
number: 104
title: Maximum Depth of Binary Tree
difficulty: Easy
url: https://leetcode.com/problems/maximum-depth-of-binary-tree/
pattern: trees
template: bottom-up
source: attempted
ask: Return the number of nodes on the longest path from the root down to a leaf.
example: "[3,9,20,null,null,15,7] gives 3. [] gives 0"
constraints: "0 <= nodes <= 10^4, -100 <= val <= 100"
---

## Pattern

- Trees, bottom-up template.
- Key: 1 + max(left, right), 0 for None.

## Clarify

- Depth counts nodes, not edges?
- Empty tree gives 0?

## Brute force

- There is none to beat. Every node has to be visited, so the plain walk is best.

```
for each leaf:
    count nodes on the path from root to it
return the biggest
```

## Optimised

```
def depth(node):
    if node is None: return 0
    return 1 + max(depth(node.left), depth(node.right))
```

## Invariant

- `depth(node)` is the number of nodes on the longest path down from node. 0 for None.

## Time and space

- Time O(n).
- Space O(h) for the call stack.

## Variation

Q: What about the minimum depth, to the nearest leaf?

A: BFS by level and stop at the first leaf. In DFS, a node with one child isn't a leaf, so don't take min with the None side. That's 111.
