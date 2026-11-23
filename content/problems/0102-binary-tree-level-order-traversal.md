---
number: 102
title: Binary Tree Level Order Traversal
difficulty: Medium
url: https://leetcode.com/problems/binary-tree-level-order-traversal/
pattern: trees
template: BFS by level
source: attempted
ask: Return the node values level by level, left to right, one list per level.
example: "[3,9,20,null,null,15,7] gives [[3],[9,20],[15,7]]"
constraints: "0 <= nodes <= 2000, -1000 <= val <= 1000"
---

## Pattern

- Trees, BFS by level template.
- Key: range(len(q)) is one level.

## Clarify

- Empty tree gives []?
- Left to right inside each level?

## Brute force

- There is none to beat. Every node is visited once anyway.
- A DFS that passes depth down also works: append to `out[depth]`.

```
def walk(node, d):
    if node is None: return
    if d == len(out): out.append([])
    out[d].append(node.val)
    walk(node.left, d + 1); walk(node.right, d + 1)
```

## Optimised

```
out = [], q = deque([root]) if root else deque()
while q:
    level = []
    for _ in range(len(q)):
        node = q.popleft()
        level.append(node.val)
        if node.left: q.append(node.left)
        if node.right: q.append(node.right)
    out.append(level)
return out
```

## Invariant

- At the top of the while, `q` holds exactly one level, left to right.

## Time and space

- Time O(n).
- Space O(n). The widest level can hold about n / 2 nodes.

## Variation

Q: What if you want only the rightmost node of each level?

A: Append `level[-1]`, or the last node popped in the inner loop. That's 199 Binary Tree Right Side View.
