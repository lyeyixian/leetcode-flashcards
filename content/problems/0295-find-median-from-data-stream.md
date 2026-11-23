---
number: 295
title: Find Median from Data Stream
difficulty: Hard
url: https://leetcode.com/problems/find-median-from-data-stream/
pattern: heap
template: two heaps
source: attempted
ask: Design addNum and findMedian. findMedian returns the middle value, or the mean of the two middles.
example: "addNum(1), addNum(2), findMedian() gives 1.5, addNum(3), findMedian() gives 2.0"
constraints: "-10^5 <= num <= 10^5, up to 5 * 10^4 calls, findMedian only after an add"
---

## Pattern

- Heap, two heaps template.
- Key: max-heap low half, min-heap high half.

## Clarify

- Return a float?
- How many calls, and which is called more often?

## Brute force

addNum O(n), findMedian O(1).

```
addNum(x): insort(arr, x)
findMedian(): middle of arr, or mean of the two middles
```

## Bottleneck

- Keeping every element in order costs O(n) per insert.
- You only need the two middle values. Split at the middle and keep each half's edge on top.

## Optimised

```
addNum(x):
    heappush(lo, -x)
    heappush(hi, -heappop(lo))
    if len(hi) > len(lo): heappush(lo, -heappop(hi))
findMedian():
    if len(lo) > len(hi): return -lo[0]
    return (-lo[0] + hi[0]) / 2
```

## Invariant

- Every value in `lo` is <= every value in `hi`.
- `lo` has the same count as `hi`, or one more.

## Time and space

- Time O(log n) per add, O(1) per median.
- Space O(n).

## Variation

Q: What if every number is in 0 to 100?

A: Keep 101 counts. findMedian walks the counts to the middle, O(100). No heaps needed.
