---
number: 973
title: K Closest Points to Origin
difficulty: Medium
url: https://leetcode.com/problems/k-closest-points-to-origin/
pattern: heap
template: top-k
source: attempted
ask: Return the k points closest to (0, 0), in any order.
example: "points = [[3,3],[5,-1],[-2,4]], k = 2 gives [[3,3],[-2,4]]"
constraints: "1 <= k <= n <= 10^4, -10^4 <= x, y <= 10^4, no tie at the kth place"
---

## Pattern

- Heap, top-k template.
- Key: max-heap of size k on distance.

## Clarify

- Any order in the output?
- Ties at the kth place? The problem says no.
- Compare x² + y², no square root needed.

## Brute force

O(n log n).

```
points.sort(key=lambda p: p[0]**2 + p[1]**2)
return points[:k]
```

## Bottleneck

- Sorting also orders the n - k points you throw away.
- Keep only k. The farthest kept is at risk, so it goes on top.

## Optimised

```
h = []
for x, y in points:
    d = x*x + y*y
    if len(h) < k: heappush(h, (-d, x, y))
    elif -d > h[0][0]: heapreplace(h, (-d, x, y))
return [[x, y] for _, x, y in h]
```

- Other way: quickselect partitions on distance. O(n) on average, but reorders the input.

## Invariant

- `h` holds the k closest points so far, the farthest of them on top.

## Time and space

- Time O(n log k).
- Space O(k).

## Variation

Q: What if the points arrive as a stream?

A: The heap still works, one point at a time. Sorting and quickselect need all the data up front.
