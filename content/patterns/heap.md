---
id: heap
name: Heap and top k
order: 8
problems: [1046, 973, 23, 295, 621]
---

## Core idea

- If you need the smallest or largest again after every change, use a heap.
- A heap keeps only the top in order. That's why it's cheaper than keeping everything sorted.
- Write one line above every heap: what it holds, and what's on top. That line also picks min or max.

## Recognise it

- "k largest", "k smallest", "k closest", "k most frequent", "kth"
- k sorted lists, arrays or matrix rows to merge
- "median" of numbers that keep arriving
- a schedule: pick the best thing allowed right now, some are blocked for a while
- things keep arriving or changing. If nothing changes after one sort, sorting is fine.

### Questions to ask

1. Do I only ever need the top, never the full order?
2. Which one is at risk of being thrown out? That one goes on top.
    - k largest: the weakest kept is at risk, so a min-heap.
    - k smallest or closest: a max-heap, with negated values.
3. How big does the heap get? The log is of that size, not of n.

### Looks right but isn't

- "kth largest" asked once on data you can reorder. Quickselect is O(n) on average. Mention it after the heap.
- The order of every element matters, like "sort by frequency". You need a full sort or a bucket sort.
- "Smallest that is at least x". A heap can't search. Use a sorted list or binary search.

## Brute force it beats

- Find the smallest or largest from scratch after every change. O(n) per step, often O(n²) total.
- Bottleneck: only one thing changed, but the scan starts over. A heap finds the new top in O(log size).

## Templates

| | top-k | k-way merge | two heaps | best ready item |
| -- | -- | -- | -- | -- |
| heap holds | the k best so far | each source's front | the smaller and larger halves | what can run now |
| on top | the weakest kept | the smallest front | the two middles | the best ready |
| size | k | k sources | n | distinct items |

### Top-k

Cue: "k largest", "k closest", "kth".

- Keep at most k. Throw out the weakest kept.
- The one at risk goes on top.

```python
h = []                         # the k largest so far, smallest on top
for x in nums:
    if len(h) < k:
        heapq.heappush(h, x)
    elif x > h[0]:
        heapq.heapreplace(h, x)
return h[0]                    # the kth largest
```

### K-way merge

Cue: k sorted lists, arrays or rows.

- The heap holds one front per source.
- Pop the smallest, push the next from the same source.
- Put a unique index in the tuple, so ties never compare the items.

```python
h = [(lst[0], i, 0) for i, lst in enumerate(lists) if lst]
heapq.heapify(h)
while h:
    val, i, j = heapq.heappop(h)
    out.append(val)
    if j + 1 < len(lists[i]):
        heapq.heappush(h, (lists[i][j + 1], i, j + 1))
```

### Two heaps

Cue: "median", the middle of a changing set.

- `lo` is the smaller half as a max-heap, `hi` is the larger half.
- Every add goes into `lo`, then `lo`'s top moves to `hi`, then rebalance so `lo` keeps the extra one.

```python
def add(x):
    heapq.heappush(lo, -x)
    heapq.heappush(hi, -heapq.heappop(lo))
    if len(hi) > len(lo):
        heapq.heappush(lo, -heapq.heappop(hi))

def median():
    return -lo[0] if len(lo) > len(hi) else (-lo[0] + hi[0]) / 2
```

### Best ready item

Cue: "schedule", "cooldown", "smash the two heaviest and put the rest back".

- A heap for what's ready now.
- A queue for what's cooling down. Everything waits the same time, so they come back in order.

```python
h = [-c for c in Counter(tasks).values()]   # ready counts, most left on top
heapq.heapify(h)
cool, t = deque(), 0                         # (count, time it's ready again)
while h or cool:
    t += 1
    if h:
        c = heapq.heappop(h) + 1
        if c: cool.append((c, t + n))
    else:
        t = cool[0][1]                       # skip the idle stretch
    if cool and cool[0][1] == t:
        heapq.heappush(h, cool.popleft()[0])
return t
```

### heapq in Python

| call | does | cost |
| -- | -- | -- |
| `h[0]` | look at the smallest | O(1) |
| `heappush(h, x)` | add one | O(log n) |
| `heappop(h)` | remove the smallest | O(log n) |
| `heapify(lst)` | make a heap in place | O(n) |
| `heapreplace(h, x)` | pop then push | O(log n) |
| `heappushpop(h, x)` | push then pop | O(log n) |

- Min-heap only. For a max-heap, push `-x`.
- `h[1]` isn't the second smallest. Only the top is in order.

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 1046 | Last Stone Weight | best ready item | max-heap, pop two, push the difference |
| 973 | K Closest Points to Origin | top-k | max-heap of size k on distance |
| 23 | Merge k Sorted Lists | k-way merge | (val, i, node), push the popped node's next |
| 295 | Find Median from Data Stream | two heaps | max-heap low half, min-heap high half |
| 621 | Task Scheduler | best ready item | heap of counts, queue of cooling tasks |

## Cost

- Time O(n log k), where k is the heap's size.
- Space O(k).
