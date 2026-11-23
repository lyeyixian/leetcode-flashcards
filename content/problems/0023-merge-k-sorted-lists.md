---
number: 23
title: Merge k Sorted Lists
difficulty: Hard
url: https://leetcode.com/problems/merge-k-sorted-lists/
pattern: heap
template: k-way merge
source: attempted
ask: Merge k sorted linked lists into one sorted list and return its head.
example: "[[1,4,5],[1,3,4],[2,6]] gives [1,1,2,3,4,4,5,6]"
constraints: "0 <= k <= 10^4, 0 <= each length <= 500, total nodes <= 10^4"
---

## Pattern

- Heap, k-way merge template.
- Key: (val, i, node), push the popped node's next.

## Clarify

- Can lists be empty, or hold empty lists?
- Reuse the nodes, or make new ones?

## Brute force

O(N · k), N = total nodes.

```
dummy = tail = ListNode()
while any list is non-empty:
    scan every head for the smallest
    tail.next = it; tail = it; advance that list
return dummy.next
```

## Bottleneck

- Each step rescans all k heads, but only one head changed.
- A heap of the heads finds the new smallest in O(log k).

## Optimised

```
h = [(l.val, i, l) for i, l in enumerate(lists) if l]; heapify(h)
dummy = tail = ListNode()
while h:
    _, i, node = heappop(h)
    tail.next = node; tail = node
    if node.next: heappush(h, (node.next.val, i, node.next))
return dummy.next
```

- Other way: merge in pairs like merge sort. log k rounds of O(N), and O(1) extra space.

## Invariant

- `h` holds the front of every list that still has nodes.
- `i` breaks ties. Without it, two equal vals compare the nodes and raise TypeError.

## Time and space

- Time O(N log k).
- Space O(k).

## Variation

Q: Why not merge them one by one into a growing result?

A: The first list's nodes get walked in every one of the k - 1 merges. That's O(N · k).
