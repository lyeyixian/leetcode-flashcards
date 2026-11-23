---
id: intervals
name: Intervals
order: 11
problems: [56, 57, 435, 452, 253]
---

## Core idea

- Things with a start and an end on one line.
- Sort them, by start or by end.
- Once sorted, each interval only needs comparing with the last one you kept, not all of them.
- Read an example with a touching pair. It decides `<` or `<=`.

## Recognise it

- a list of `[start, end]`: meetings, bookings, ranges, balloons
- "merge them", "how many overlap at once", "fewest to remove so none overlap", "fewest points that hit them all"
- a range of numbers on a line, even if it isn't called an interval

### Questions to ask

1. Sort by start or by end?
    - Combining or counting: by start. You walk along the line in order.
    - Choosing which to keep, or where to put points: by end. The earliest end leaves the most room.
2. Do touching intervals overlap here? [1,2] and [2,3].
3. What do I keep while I walk? The last merged interval, the last kept end, a heap of room ends.
4. Two intervals [a,b] and [c,d] overlap when `a <= d and c <= b`. Both have to hold.

### Looks right but isn't

- Keeping the first of each clash when sorted by start (435). [1,10] blocks the short ones. Sort by end.
- Keeping the shortest first. [1,5], [4,7], [6,10] picks [4,7], which blocks both others.
- Intervals with weights, like "max profit from jobs" (1235). Picking by end isn't enough. That's DP with binary search.

## Brute force it beats

- Compare every pair, often again until nothing changes. O(n²) or worse.
- Bottleneck: most pairs are far apart and can't overlap. After sorting, only neighbours matter.

## Templates

| | merge into the last | insert in three parts | keep the earliest end | heap of room ends |
| -- | -- | -- | -- | -- |
| sort by | start | already sorted | end | start |
| keep while walking | `ans[-1]` | the new interval, growing | last kept end | min-heap of ends |
| answer | `ans` | three parts joined | n - kept, or kept | `len(heap)` |

### Merge into the last

Cue: "merge all overlapping intervals".

- Sort by start.
- Overlaps the last merged one: stretch its end with `max`. It may sit inside.
- Otherwise append.

```python
intervals.sort()
ans = []
for start, end in intervals:
    if ans and ans[-1][1] >= start:
        ans[-1][1] = max(ans[-1][1], end)
    else:
        ans.append([start, end])
```

### Insert in three parts

Cue: already sorted and non-overlapping, add one more.

- Copy everything that ends before the new one starts.
- Merge everything that starts at or before the new one ends.
- Append the new one, then copy the rest.

```python
while i < n and intervals[i][1] < ns:
    ans.append(intervals[i]); i += 1
while i < n and intervals[i][0] <= ne:
    ns = min(ns, intervals[i][0]); ne = max(ne, intervals[i][1]); i += 1
ans.append([ns, ne])
ans.extend(intervals[i:])
```

### Keep the earliest end

Cue: "fewest to remove so none overlap", "most you can keep", "fewest arrows".

- Sort by end. Start `prev_end` at `-inf`.
- Keep an interval when it doesn't touch the last kept one.
- Why it's safe: swap the first interval of any best answer for the one that ends earliest. No new clash, same count.

```python
intervals.sort(key=lambda x: x[1])
keep, prev_end = 0, float("-inf")
for start, end in intervals:
    if start >= prev_end:      # > for arrows, touching shares one
        keep += 1
        prev_end = end
```

### Heap of room ends

Cue: "how many rooms", "platforms", the most overlapping at one point.

- Sort by start.
- The heap holds one end time per room.
- If the earliest end is free, reuse that room. Then push this meeting's end.

```python
intervals.sort()
heap = []
for start, end in intervals:
    if heap and start >= heap[0]:
        heapq.heappop(heap)
    heapq.heappush(heap, end)
return len(heap)
```

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 56 | Merge Intervals | merge into the last | sort by start, max on the end |
| 57 | Insert Interval | insert in three parts | before, overlapping, after |
| 435 | Non-overlapping Intervals | keep the earliest end | start >= prev end keeps it, answer n - kept |
| 452 | Minimum Number of Arrows to Burst Balloons | keep the earliest end | start > prev end needs a new arrow |
| 253 | Meeting Rooms II | heap of room ends | reuse the room that frees first |

## Cost

- Time O(n log n) for the sort. The walk is O(n).
- Space O(n) for the sort and the output.
