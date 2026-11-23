---
number: 621
title: Task Scheduler
difficulty: Medium
url: https://leetcode.com/problems/task-scheduler/
pattern: heap
template: best ready item
source: attempted
ask: Tasks are letters, one unit each. The same letter must be at least n units apart. Return the fewest units to finish all.
example: "tasks = [A,A,A,B,B,B], n = 2 gives 8, from A B idle A B idle A B"
constraints: "1 <= len(tasks) <= 10^4, uppercase A to Z, 0 <= n <= 100"
---

## Pattern

- Heap, best ready item template.
- Key: heap of counts, queue of cooling tasks.

## Clarify

- n is the cooldown, not the task count?
- Any order allowed?
- Return the time, not the schedule?

## Brute force

O(T · 26), T = len(tasks). Simulate each unit.

```
while tasks left:
    pick any letter whose cooldown is over, run it
    otherwise idle
    tick every letter's cooldown down by one
```

## Bottleneck

- Picking any ready letter can leave the busiest one stuck at the end with idle gaps.
- Always run the ready letter with the most left. A heap gives it in O(log 26).

## Optimised

```
h = [-c for c in Counter(tasks).values()]; heapify(h)
cool, t = deque(), 0
while h or cool:
    t += 1
    if h:
        c = heappop(h) + 1
        if c: cool.append((c, t + n))
    else:
        t = cool[0][1]
    if cool and cool[0][1] == t:
        heappush(h, cool.popleft()[0])
return t
```

- Other way: a formula. `(max_count - 1) * (n + 1) + how many letters have max_count`, or len(tasks) if that's bigger.

## Invariant

- `h` holds tasks ready now, the most left on top. `cool` holds tasks waiting, in the order they become ready.

## Time and space

- Time O(T log 26), so O(T).
- Space O(26).

## Variation

Q: Why does the idle jump come before the release check?

A: If you release first, the next `t += 1` skips past the release time. Then `==` never matches and the loop never ends.
