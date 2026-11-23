---
number: 206
title: Reverse Linked List
difficulty: Easy
url: https://leetcode.com/problems/reverse-linked-list/
pattern: linked-list
template: reverse in place
source: attempted
ask: Reverse a singly linked list and return the new head.
example: "[1,2,3,4,5] gives [5,4,3,2,1]. [] gives []"
constraints: "0 <= nodes <= 5000, -5000 <= val <= 5000"
---

## Pattern

- Linked list, reverse in place template.
- Key: save, flip, step, step.

## Clarify

- Can the list be empty?
- Loop or recursion? Recursion uses O(n) stack.

## Brute force

O(n) time, O(n) space.

```
vals = copy every value into an array
rebuild a new list from vals in reverse
```

## Bottleneck

- No repeated work. The array costs O(n) space.
- Flip each arrow as you walk instead.

## Optimised

```
prev, cur = None, head
while cur:
    nxt = cur.next
    cur.next = prev
    prev = cur
    cur = nxt
return prev
```

## Invariant

- `prev` is the head of the reversed part. `cur` is the head of the part not yet reversed.

## Time and space

- Time O(n).
- Space O(1).

## Variation

Q: What breaks if you flip before you save?

A: `cur.next` already points back, so `nxt` picks up the arrow you just flipped. The rest of the list is lost.
