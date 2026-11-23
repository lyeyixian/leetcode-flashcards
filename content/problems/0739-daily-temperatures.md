---
number: 739
title: Daily Temperatures
difficulty: Medium
url: https://leetcode.com/problems/daily-temperatures/
pattern: stack
template: resolve on pop
source: attempted
ask: For each day, return how many days until a warmer day. 0 if none comes.
example: "[73,74,75,71,69,72,76,73] gives [1,1,4,2,1,1,0,0]"
constraints: "1 <= n <= 10^5, 30 <= temperatures[i] <= 100"
---

## Pattern

- Monotonic stack, resolve on pop template.
- Key: pop days colder than today, answer i - j.

## Clarify

- Strictly warmer? An equal day doesn't count.
- 0 when no warmer day comes?
- n up to 10^5, so aim for O(n) or O(n log n).

## Brute force

O(n²).

```
for i in range(n):
    for j in range(i + 1, n):
        if t[j] > t[i]:
            ans[i] = j - i; break
```

## Bottleneck

- Each day scans forward over the same future days again.
- Days still waiting can sit on a stack. Each new day answers every colder one on top.

## Optimised

```
ans = [0] * n, stack = []
for i, x in enumerate(t):
    while stack and t[stack[-1]] < x:
        j = stack.pop()
        ans[j] = i - j
    stack.append(i)
return ans
```

## Invariant

- The stack holds days with no warmer day yet, temperatures not going up from bottom to top.
- When `i` pops `j`, no day between them was warmer, or `j` would already be gone. So `i - j` is final.

## Time and space

- Time O(n). Each index is pushed and popped once.
- Space O(n).

## Variation

Q: What if the temperatures wrap around, so after the last day comes the first?

A: Loop twice over the indices, pushing only in the first pass. That's 503 Next Greater Element II.
