---
number: 853
title: Car Fleet
difficulty: Medium
url: https://leetcode.com/problems/car-fleet/
pattern: stack
template: the stack is the answer
source: attempted
ask: Cars drive one lane to target and can't pass. A car that catches up joins that fleet. Return how many fleets arrive.
example: "target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3] gives 3"
constraints: "1 <= n <= 10^5, 0 < target <= 10^6, 0 <= position[i] < target, positions unique, 0 < speed[i] <= 10^6"
---

## Pattern

- Monotonic stack, the stack is the answer template. Sort first.
- Key: sort by position, a car that arrives no later joins the fleet ahead.

## Clarify

- Positions unique? Speeds above 0?
- A car catching up exactly at target joins that fleet?
- n up to 10^5, so O(n log n) is fine.

## Brute force

O(n²).

```
sort cars by position
time[i] = (target - pos[i]) / speed[i]
for each car i:
    if any car ahead of i has time >= time[i]:
        i joins a fleet ahead
    else: fleets += 1
```

## Bottleneck

- Each car scans every car ahead, but only the slowest fleet ahead matters.
- Go from the car nearest target back. Keep the arrival times of the fleets so far.

## Optimised

```
stack = []
for pos, s in sorted(zip(position, speed), reverse=True):
    t = (target - pos) / s
    if not stack or t > stack[-1]:
        stack.append(t)
return len(stack)
```

- Better: only the top is ever read, so keep one `slowest` time and a count. O(1) extra space.

## Invariant

- Walking from the front, the stack holds each fleet's arrival time, going up.
- A car with time `<= stack[-1]` catches the fleet ahead before target and joins it.

## Time and space

- Time O(n log n) for the sort.
- Space O(n) for the pairs, O(1) extra with the running `slowest`.

## Variation

Q: Why a float time and not `ceil`?

A: Rounding up can make two cars look tied when one actually arrives later. Then they'd wrongly merge.
