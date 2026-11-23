---
id: linked-list
name: Linked list
order: 6
problems: [206, 19, 142, 143, 448, 287]
---

## Core idea

- An array lets you jump to an index for free. A linked list doesn't.
- Use two or three pointers to do what indexing would do, without extra space.
- The brute force is always "copy the nodes into an array". That pays O(n) space to get indexing back.
- The same tricks work on an array whose values name slots, like values in 1..n.

## Recognise it

- the input is a linked list and the follow-up asks for O(1) memory
- "the middle", "the kth from the end"
- "is there a cycle", "where does the cycle start"
- anything you'd want to walk backwards: reverse all, a range, k at a time, compare both ends
- the head might change: remove nodes, insert at the front, merge into a new list
- an array of values in 1..n, O(1) extra space, missing or repeated values

### Questions to ask

1. What would the array copy give me? The middle, going backwards, a way to spot a repeat?
2. Which template gets that one thing back without the array?
3. Might the head change? Then add a dummy.

### Looks right but isn't

- Index as a hash key when values go outside 1..n, say up to 10^9. There is no slot to mark. Use a set.
- Marking by negating when you're told not to change the input (287). Follow `i -> nums[i]` instead.
- A set of seen nodes for a cycle. Correct, but O(n) memory. Fine as the brute force, not after "now O(1) memory".

## Brute force it beats

- Copy into an array, work on the array. O(n) time, O(n) space.
- Bottleneck: the space, not the time. Each template gets back one thing the array gave you.

## Templates

| | dummy head | fast and slow | reverse in place | index as a hash key |
| -- | -- | -- | -- | -- |
| gets back | a node before the head | the middle, kth from end, cycle | walking backwards | a seen set |
| cue | head might change | middle, cycle | reverse, compare from both ends | values in 1..n |

### Dummy head

Cue: remove nodes, insert at the front, build a new list.

- `dummy = ListNode(0, head)`. Hold the node before the one you're looking at.
- Remove: `prev.next = prev.next.next`, prev stays.
- Return `dummy.next`, not `head`, which might be gone.

```python
dummy = ListNode(0, head)
prev = dummy
while prev.next:
    if should_remove(prev.next):
        prev.next = prev.next.next
    else:
        prev = prev.next
return dummy.next
```

### Fast and slow

Cue: "the middle", "kth from the end", "cycle".

- `while fast and fast.next` stops slow on the second middle of an even list.
- `while fast.next and fast.next.next` stops slow on the first middle.
- Cycle: move first, then compare with `is`, since both start on head.
- Cycle start: when they meet, put one back on head and step both by one. They meet at the start.
- kth from the end: fast moves k + 1 from the dummy first, then both move until fast is None.

```python
slow = fast = head
while fast and fast.next:
    slow, fast = slow.next, fast.next.next
    if slow is fast:               # cycle
        slow = head
        while slow is not fast:
            slow, fast = slow.next, fast.next
        return slow                # cycle start
```

### Reverse in place

Cue: reverse all, a range, k at a time, the second half to compare or weave.

- prev starts at None, because the old head becomes the tail.
- Save, flip, step prev, step cur, in that order.
- Return prev, since cur ends on None.

```python
prev, cur = None, head
while cur:
    nxt = cur.next     # save
    cur.next = prev    # flip
    prev = cur         # step prev
    cur = nxt          # step cur
return prev
```

### Index as a hash key

Cue: values in 1..n, O(1) extra space.

- Value `v` goes with index `v - 1`.
- Mark: make `nums[v - 1]` negative. Unmarked slots are missing values.
- Follow: treat `i -> nums[i]` as a next pointer, then run the cycle start. Changes nothing.
- Read with `abs()`, since an earlier step may have flipped it.

```python
for x in nums:                     # mark
    i = abs(x) - 1
    nums[i] = -abs(nums[i])
return [i + 1 for i, x in enumerate(nums) if x > 0]
```

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 206 | Reverse Linked List | reverse in place | save, flip, step, step |
| 19 | Remove Nth Node From End of List | fast and slow | dummy, fast goes n + 1 ahead |
| 142 | Linked List Cycle II | fast and slow | after they meet, restart one from head |
| 143 | Reorder List | reverse in place | find middle, reverse second half, weave |
| 448 | Find All Numbers Disappeared in an Array | index as a hash key | negate nums[v - 1], positives are missing |
| 287 | Find the Duplicate Number | index as a hash key | follow i -> nums[i], the cycle start is the repeat |

## Cost

- Time O(n). Each pointer walks the list a fixed number of times.
- Space O(1). A recursive version uses O(n) call stack.
