---
number: 1011
title: Capacity To Ship Packages Within Days
difficulty: Medium
url: https://leetcode.com/problems/capacity-to-ship-packages-within-days/
pattern: binary-search
template: search the answer
source: attempted
ask: Ship packages in order, one load per day, each load no heavier than the capacity. Return the smallest capacity that ships all within days.
example: "weights = [1,2,3,4,5,6,7,8,9,10], days = 5 gives 15"
constraints: "1 <= days <= n <= 5 * 10^4, 1 <= weights[i] <= 500"
---

## Pattern

- Binary search, search the answer template.
- Key: days(cap) <= days, cap from max to sum.

## Clarify

- Packages must keep their order?
- Weights all positive?
- Can a day ship nothing? It doesn't help, so no.

## Brute force

O(n · m), m = sum(weights).

```
for cap in range(max(weights), sum(weights) + 1):
    if days_needed(cap) <= days:
        return cap
```

## Bottleneck

- `days_needed` is a greedy O(n) pass and is needed. The outer loop steps one at a time.
- More capacity never needs more days, so the condition reads F F F T T T.

## Optimised

```
lo, hi = max(weights), sum(weights)
while lo < hi:
    mid = (lo + hi) // 2
    need, load = 1, 0
    for w in weights:
        if load + w > mid: need += 1; load = 0
        load += w
    if need <= days: hi = mid
    else: lo = mid + 1
return lo
```

## Invariant

- The answer is inside `[lo, hi]`. Below `max(weights)` the heaviest package can't ship. `sum(weights)` ships all in one day.

## Time and space

- Time O(n log m), m = sum(weights).
- Space O(1).

## Variation

Q: What if you split an array into k parts and minimise the largest part sum?

A: Same code. Capacity is the largest part, days is k. That's 410 Split Array Largest Sum.
