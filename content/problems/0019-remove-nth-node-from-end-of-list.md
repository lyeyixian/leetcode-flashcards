---
number: 19
title: Remove Nth Node From End of List
difficulty: Medium
url: https://leetcode.com/problems/remove-nth-node-from-end-of-list/
pattern: linked-list
template: fast and slow
source: attempted
ask: Remove the nth node from the end of the list and return its head, in one pass.
example: "[1,2,3,4,5], n = 2 gives [1,2,3,5]. [1], n = 1 gives []"
constraints: "1 <= nodes <= 30, 1 <= n <= nodes"
---

## Pattern

- Linked list, fast and slow template, with a dummy head.
- Key: dummy, fast goes n + 1 ahead.

## Clarify

- n is always valid?
- What if n is the length, so the head goes? The dummy covers it.

## Brute force

Two passes, O(n).

```
length = count the nodes
walk to node length - n - 1 from a dummy
skip its next
```

## Bottleneck

- Two passes read the list twice. A gap of n + 1 between two pointers finds the spot in one.

## Optimised

```
dummy = ListNode(0, head)
slow = fast = dummy
for _ in range(n + 1): fast = fast.next
while fast:
    slow, fast = slow.next, fast.next
slow.next = slow.next.next
return dummy.next
```

## Invariant

- fast is always n + 1 nodes ahead of slow. When fast is None, slow is just before the node to remove.

## Time and space

- Time O(n). Not faster than two passes in big O, it just reads the list once.
- Space O(1).

## Variation

Q: Why start both at the dummy and not at head?

A: When n is the length, the node to remove is head. slow must sit before it, and only the dummy is there.
