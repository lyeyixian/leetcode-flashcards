---
number: 1046
title: Last Stone Weight
difficulty: Easy
url: https://leetcode.com/problems/last-stone-weight/
pattern: heap
template: best ready item
source: attempted
ask: Each turn, smash the two heaviest stones. Equal ones both go, otherwise the bigger becomes the difference. Return the last weight, or 0.
example: "[2,7,4,1,8,1] gives 1"
constraints: "1 <= n <= 30, 1 <= stones[i] <= 1000"
---

## Pattern

- Heap, best ready item template. Your first max-heap in Python.
- Key: max-heap, pop two, push the difference.

## Clarify

- Return 0 if nothing is left?
- n is only 30 here. Would they push it to 10^5?

## Brute force

O(n²).

```
while len(stones) > 1:
    remove the two largest y >= x by scanning
    if y != x: stones.append(y - x)
return stones[0] if stones else 0
```

## Bottleneck

- Each turn rescans for the two heaviest, but only one stone changed.
- A max-heap finds the heaviest again in O(log n).

## Optimised

```
h = [-s for s in stones]; heapify(h)
while len(h) > 1:
    y, x = -heappop(h), -heappop(h)
    if y != x: heappush(h, -(y - x))
return -h[0] if h else 0
```

## Invariant

- `h` holds every stone left, negated, so the heaviest is on top.

## Time and space

- Time O(n log n).
- Space O(n).

## Variation

Q: What if you can pick any two stones, and want the smallest possible last weight?

A: It's no longer greedy. Split the stones into two groups with sums as close as possible. That's 1049, a knapsack.
