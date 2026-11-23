---
number: 142
title: Linked List Cycle II
difficulty: Medium
url: https://leetcode.com/problems/linked-list-cycle-ii/
pattern: linked-list
template: fast and slow
source: attempted
ask: Return the node where the cycle begins, or None if there's no cycle. Don't change the list. O(1) memory.
example: "[3,2,0,-4] with the tail linked to index 1 gives the node at index 1"
constraints: "0 <= nodes <= 10^4"
---

## Pattern

- Linked list, fast and slow template, Floyd's cycle detection.
- Key: after they meet, restart one from head.

## Clarify

- Can the list be empty or have no cycle? Return None.
- Must not change the list?
- O(1) memory, so no set of seen nodes.

## Brute force

O(n) time, O(n) space.

```
seen = set()
while head:
    if head in seen: return head
    seen.add(head); head = head.next
return None
```

## Bottleneck

- The set costs O(n) space. Two pointers at different speeds find the cycle with none.

## Optimised

```
slow = fast = head
while fast and fast.next:
    slow, fast = slow.next, fast.next.next
    if slow is fast:
        slow = head
        while slow is not fast:
            slow, fast = slow.next, fast.next
        return slow
return None
```

## Invariant

- Let a = steps from head to the cycle start, b = steps into the cycle where they meet.
- slow walked a + b, fast walked 2(a + b). The extra a + b is a whole number of loops.
- So a more steps from the meeting point lands on the cycle start, same as a steps from head.

## Time and space

- Time O(n).
- Space O(1).

## Variation

Q: What if you only need to know whether there is a cycle?

A: Stop at `slow is fast` and return True. That's 141 Linked List Cycle.
