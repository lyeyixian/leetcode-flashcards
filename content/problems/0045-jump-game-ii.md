---
number: 45
title: Jump Game II
difficulty: Medium
url: https://leetcode.com/problems/jump-game-ii/
pattern: greedy
template: jump levels
source: attempted
ask: Same jumps as Jump Game, and the end is always reachable. Return the fewest jumps to reach the last index.
example: "[2,3,1,1,4] gives 2, from 0 to 1 to 4. [0] gives 0"
constraints: "1 <= n <= 10^4, 0 <= nums[i] <= 1000, the last index is reachable"
---

## Pattern

- Greedy, jump levels template.
- Key: levels are solid ranges.

## Clarify

- Always reachable?
- One element returns 0?

## Brute force

- BFS over indexes with a queue and a visited set, one level per jump. O(n²) edges in the worst case.

```
q, seen, jumps = deque([0]), {0}, 0
while q:
    for _ in range(len(q)):
        i = q.popleft()
        if i == n - 1: return jumps
        for j in range(i + 1, min(n, i + nums[i] + 1)):
            if j not in seen: seen.add(j); q.append(j)
    jumps += 1
```

## Bottleneck

- Each level is a solid range of indexes, same reason as Jump Game.
- Its two ends replace the queue and the visited set.

## Optimised

```
left = right = jumps = 0
while right < n - 1:
    far = max(i + nums[i] for i in range(left, right + 1))
    left, right = right + 1, far
    jumps += 1
return jumps
```

## Invariant

- `left..right` is exactly the indexes `jumps` jumps away.
- After j jumps you can't be past level j's right. So the first level that holds the last index gives the fewest.

## Time and space

- Time O(n). Each index is scanned once across all levels.
- Space O(1).

## Variation

Q: Why not jump as far as you can from each spot?

A: On [2,3,1,1,4] it goes 0, 2, 3, 4, three jumps. Landing on 1 reaches further next time.
