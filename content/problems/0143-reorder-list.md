---
number: 143
title: Reorder List
difficulty: Medium
url: https://leetcode.com/problems/reorder-list/
pattern: linked-list
template: reverse in place
source: attempted
ask: Turn L0, L1, ..., Ln into L0, Ln, L1, Ln-1, L2, ... by changing next pointers. Return nothing.
example: "[1,2,3,4,5] gives [1,5,2,4,3]"
constraints: "1 <= nodes <= 5 * 10^4"
---

## Pattern

- Linked list. Fast and slow to find the middle, reverse in place for the second half, then weave.
- Key: find middle, reverse second half, weave.

## Clarify

- Change pointers, not values?
- In place, return nothing?

## Brute force

O(n) time, O(n) space.

```
nodes = copy every node into an array
i, j = 0, n - 1
link nodes[i] -> nodes[j] -> nodes[i + 1] ... until they meet
set the last next to None
```

## Bottleneck

- The array gives you walking backwards from the end. Reversing the second half gives you the same thing in O(1) space.

## Optimised

```
slow = fast = head
while fast.next and fast.next.next:
    slow, fast = slow.next, fast.next.next
second = reverse(slow.next)
slow.next = None
first = head
while second:
    n1, n2 = first.next, second.next
    first.next = second
    second.next = n1
    first, second = n1, n2
```

## Invariant

- After the cut, the first half is as long as the second, or one longer. So the weave runs out on `second` first.

## Time and space

- Time O(n).
- Space O(1).

## Variation

Q: What if you need to check whether the list is a palindrome?

A: Same first two steps. Then walk both halves and compare values instead of weaving. That's 234.
