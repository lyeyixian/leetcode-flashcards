---
number: 98
title: Validate Binary Search Tree
difficulty: Medium
url: https://leetcode.com/problems/validate-binary-search-tree/
pattern: trees
template: top-down
source: attempted
ask: Return whether the tree is a valid BST. Left subtree strictly less, right subtree strictly greater, at every node.
example: "[2,1,3] gives true. [5,4,6,null,null,3,7] gives false, 3 is in 5's right subtree"
constraints: "1 <= nodes <= 10^4, -2^31 <= val <= 2^31 - 1"
---

## Pattern

- Trees, top-down template, carrying a range.
- Key: pass the allowed range down.

## Clarify

- Strictly less and greater, so no duplicates?
- Values can hit the 32-bit limits, so start the range at infinity?

## Brute force

O(n²) on a chain.

```
for each node:
    if max(node.left subtree) >= node.val: return False
    if min(node.right subtree) <= node.val: return False
return True
```

## Bottleneck

- Each node rescans its subtrees for max and min.
- A node only needs the limits set by its ancestors. Pass them down.

## Optimised

```
def ok(node, lo, hi):
    if node is None: return True
    if not (lo < node.val < hi): return False
    return ok(node.left, lo, node.val) and ok(node.right, node.val, hi)
return ok(root, -inf, inf)
```

- Other way: inorder walk, and check each value is bigger than the one before.

## Invariant

- `(lo, hi)` is the tightest range every ancestor allows for this node.
- Going left, the node becomes the upper limit. Going right, it becomes the lower limit.

## Time and space

- Time O(n).
- Space O(h).

## Variation

Q: What if you need the kth smallest value in a BST?

A: Inorder walk gives sorted order. Stop at the kth node. That's 230.
