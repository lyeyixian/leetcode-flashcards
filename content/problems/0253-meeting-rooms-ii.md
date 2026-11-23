---
number: 253
title: Meeting Rooms II
difficulty: Medium
url: https://leetcode.com/problems/meeting-rooms-ii/
pattern: intervals
template: heap of room ends
source: attempted
ask: Each meeting is [start, end]. Return the fewest rooms so no two meetings share a room at once. Ending at 10 frees it for one starting at 10.
example: "[[0,30],[5,10],[15,20]] gives 2. [[1,5],[5,10],[10,15]] gives 1"
constraints: "1 <= n <= 10^4, 0 <= start < end <= 10^6"
---

## Pattern

- Intervals, heap of room ends template.
- Key: reuse the room that frees first.

## Clarify

- A meeting ending at 10 frees the room for one starting at 10?
- Is the input sorted? No.

## Brute force

O(n²).

```
for each meeting i:
    count meetings running at start_i
    best = max(best, count)
```

## Bottleneck

- Each start recounts from zero. Between two starts only the new meeting joins and the finished ones leave.
- A min-heap of end times tells you who has left.

## Optimised

```
intervals.sort()
heap = []
for s, e in intervals:
    if heap and s >= heap[0]: heappop(heap)
    heappush(heap, e)
return len(heap)
```

- Other way: sort starts and ends apart, walk both, +1 on a start, -1 on an end. Keep the max.

## Invariant

- The heap holds one end per room opened. It never shrinks, so its size is the rooms opened.
- A room only opens when every room is busy. So that many meetings run at once, and no schedule uses fewer.

## Time and space

- Time O(n log n).
- Space O(n).

## Variation

Q: What if you only need to know whether one room is enough (252)?

A: Sort by start and check each meeting starts at or after the last one ends.
