---
number: 155
title: Min Stack
difficulty: Medium
url: https://leetcode.com/problems/min-stack/
pattern: stack
template: matching
source: attempted
ask: Design a stack with push, pop, top and getMin, all O(1).
example: "push(-2), push(0), push(-3), getMin() gives -3, pop(), top() gives 0, getMin() gives -2"
constraints: "-2^31 <= val <= 2^31 - 1, at most 3 * 10^4 calls, pop, top and getMin only on a non-empty stack"
---

## Pattern

- Stack. Each slot stores a bit more than the value.
- Key: store the min so far with each value.

## Clarify

- Are pop, top and getMin only called on a non-empty stack?
- Every operation O(1)?
- Duplicates of the min allowed?

## Brute force

getMin O(n).

```
def getMin():
    return min(stack)
```

- Or keep one `min` variable. getMin is O(1), but a pop of the min needs an O(n) rescan.

## Bottleneck

- getMin rescans values that haven't changed.
- A slot's min only depends on the values below it, and those never change while the slot exists.

## Optimised

```
push(val): stack.append((val, min(val, stack[-1][1]) if stack else val))
pop():     stack.pop()
top():     return stack[-1][0]
getMin():  return stack[-1][1]
```

## Invariant

- `stack[i][1]` is the min of `stack[0..i]`. A stack only changes at the top, so it never goes stale.

## Time and space

- Time O(1) for every operation.
- Space O(n).

## Variation

Q: Why doesn't this trick work for a queue?

A: A queue removes from the front, which changes what is below every slot. The stored mins go stale. Use a monotonic deque instead.
