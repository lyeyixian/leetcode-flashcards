---
number: 134
title: Gas Station
difficulty: Medium
url: https://leetcode.com/problems/gas-station/
pattern: greedy
template: running best
source: attempted
ask: Stations on a circle give gas[i], and driving to the next costs cost[i]. Return the start that completes the circle, or -1.
example: "gas = [1,2,3,4,5], cost = [3,4,5,1,2] gives 3"
constraints: "1 <= n <= 10^5, 0 <= gas[i], cost[i] <= 10^4, the answer is unique if it exists"
---

## Pattern

- Greedy, running best template.
- Key: tank below 0 at i, restart at i + 1. Total < 0 means -1.

## Clarify

- The tank starts empty?
- If an answer exists, it's unique?

## Brute force

O(n²).

```
for s in range(n):
    tank = 0
    for k in range(n):
        i = (s + k) % n
        tank += gas[i] - cost[i]
        if tank < 0: break
    else: return s
return -1
```

## Bottleneck

- A start s that fails at i rules out every start up to i. Each of them reaches i with no more gas than s did.
- So try i + 1 next, never go back.

## Optimised

```
if sum(gas) < sum(cost): return -1
tank, start = 0, 0
for i in range(n):
    tank += gas[i] - cost[i]
    if tank < 0: tank = 0; start = i + 1
return start
```

## Invariant

- `tank` is the gas left driving from `start` to here, and it's never been below 0 on the way.
- Total >= 0 means the part after `start` earns enough to pay for the part before it.

## Time and space

- Time O(n).
- Space O(1).

## Variation

Q: Why no check that the last start really gets back around?

A: The failed stretch before `start` adds up to some deficit. The total is >= 0, so the stretch from `start` to the end covers it.
