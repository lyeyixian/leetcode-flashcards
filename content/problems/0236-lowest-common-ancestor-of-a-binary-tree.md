---
number: 236
title: Lowest Common Ancestor of a Binary Tree
difficulty: Medium
url: https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/
pattern: trees
template: bottom-up
source: attempted
ask: Return the lowest node that has both p and q in its subtree. A node counts as in its own subtree.
example: "[3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4 gives 5"
constraints: "2 <= nodes <= 10^5, values unique, p != q, both are in the tree"
---

## Pattern

- Trees, bottom-up template. The return value means more than one thing.
- Key: return what you found below, both sides means here.

## Clarify

- Are both p and q always in the tree?
- Return the node, not its value?
- It's a plain binary tree, not a BST?

## Brute force

O(n²) on a chain.

```
for each node, top-down:
    if contains(node, p) and contains(node, q):
        keep it as the latest candidate
return the deepest candidate
```

## Bottleneck

- `contains` walks the same subtrees again for every ancestor.
- One bottom-up pass can say what each subtree holds.

## Optimised

```
def lca(node):
    if node is None or node is p or node is q:
        return node
    l, r = lca(node.left), lca(node.right)
    if l and r: return node
    return l or r
```

- Middle step: return a tuple `(lca, has_p, has_q)`. Then notice the parent treats some states the same and merge them.

## Invariant

- `lca(node)` returns None, p, q, or the answer, whichever it found below.
- Returning p early is safe. If q is under p, the answer is p anyway.

## Time and space

- Time O(n).
- Space O(h).

## Variation

Q: What if p or q might not be in the tree (1644)?

A: Returning p early only means "found p". Go back to the tuple and check both flags at the root.
