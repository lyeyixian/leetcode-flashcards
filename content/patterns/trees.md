---
id: trees
name: Trees
order: 7
problems: [104, 543, 236, 98, 102]
---

## Core idea

- Decide which way information flows.
    - Bottom to top: a node needs a fact about its whole subtree.
    - Top to bottom: a node needs something from the path above it.
    - Level by level: the question is about depth or "closest to the root".
- Usually recursion. Write two things first: the base case, and one line on what the function returns.

## Recognise it

- height, size, sum, balanced, diameter, "is p under here"
- allowed range, running sum, max so far on the path, depth
- "level", "right side view", "zigzag", "shallowest leaf"
- BST: sorted order means inorder. A value tells you which side to go, so walk one side only.

### Questions to ask

1. Can a node's answer be built from its children's answers? Then bottom-up.
2. Does a node need something its parent hands down? Then top-down.
3. Is it about levels or distance from the root? Then BFS.
4. Can the answer use both sides while the parent can only use one? Then return one thing and record another.

### Looks right but isn't

- Comparing each BST node only with its children (98). A node must fit every ancestor's limit. Pass a range down.
- Every node has to be looked at anyway (104, 102). The plain walk is already best, nothing to optimise.

## Brute force it beats

- Ask every node a question about its whole subtree from scratch. O(n²) on a chain.
- Bottleneck: the subtree walks overlap. Compute each subtree's answer once and hand it up, or pass what's needed down.

## Templates

| | bottom-up | top-down | BFS by level |
| -- | -- | -- | -- |
| order | postorder | preorder | level order |
| info flows | children to parent | parent to children | one level at a time |
| state | the return value | an argument | the queue |
| cue | a fact about the subtree | a rule from the path above | "level", "closest" |

### Bottom-up

Cue: height, diameter, balanced, "is p under here".

- Contract first: what `solve(node)` returns, and what it returns for None.
- Ask both children, then combine.
- If the answer uses both sides but the parent can use only one, return one, record the other with `nonlocal`.

```python
best = 0
def height(node):
    # nodes on the longest path down from node, 0 for None
    nonlocal best
    if node is None:
        return 0
    l, r = height(node.left), height(node.right)
    best = max(best, l + r)    # record: the path that turns here
    return 1 + max(l, r)       # return: one side, for the parent
```

### Top-down

Cue: allowed range, running sum, max so far.

- The state goes down as an argument.
- Check the node, then pass each child its updated state.
- Often mixed: the range goes down, "valid" comes back up.

```python
def ok(node, lo, hi):
    # every value under node is strictly between lo and hi
    if node is None:
        return True
    if not (lo < node.val < hi):
        return False
    return ok(node.left, lo, node.val) and ok(node.right, node.val, hi)
```

### BFS by level

Cue: "level order", "right side view", "zigzag", "minimum depth".

- `range(len(q))` is read once, so the inner loop is exactly one level.
- `popleft`. Children go to the back for the next level.

```python
q = deque([root]) if root else deque()
while q:
    level = []
    for _ in range(len(q)):
        node = q.popleft()
        level.append(node.val)
        if node.left: q.append(node.left)
        if node.right: q.append(node.right)
    out.append(level)
```

### Traversal orders

| order | visit | use it for |
| -- | -- | -- |
| preorder | node, left, right | top-down, copy or serialise a tree |
| inorder | left, node, right | BST, values come out sorted |
| postorder | left, right, node | bottom-up, height, diameter |
| level order | level by level | BFS with a queue |

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 104 | Maximum Depth of Binary Tree | bottom-up | 1 + max(left, right), 0 for None |
| 543 | Diameter of Binary Tree | bottom-up | return the height, record left + right |
| 236 | Lowest Common Ancestor of a Binary Tree | bottom-up | return what you found below, both sides means here |
| 98 | Validate Binary Search Tree | top-down | pass the allowed range down |
| 102 | Binary Tree Level Order Traversal | BFS by level | range(len(q)) is one level |

## Cost

- Time O(n). Every node is visited once.
- Space O(h) for recursion, h up to n on a chain. BFS is O(n), since the widest level can hold about n / 2.
