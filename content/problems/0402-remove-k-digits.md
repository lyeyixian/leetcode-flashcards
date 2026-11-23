---
number: 402
title: Remove K Digits
difficulty: Medium
url: https://leetcode.com/problems/remove-k-digits/
pattern: stack
template: the stack is the answer
source: attempted
ask: num is a number as a string. Remove exactly k digits to make it as small as possible. No leading zeros, and "0" if empty.
example: "num = \"1432219\", k = 3 gives \"1219\". num = \"10200\", k = 1 gives \"200\""
constraints: "1 <= k <= len(num) <= 10^5, no leading zeros except \"0\" itself"
---

## Pattern

- Monotonic stack, the stack is the answer template.
- Key: pop a bigger digit in front of a smaller one.

## Clarify

- Exactly k, not at most k?
- Strip leading zeros, return "0" when nothing is left?
- n up to 10^5, so O(nk) can be too slow.

## Brute force

O(nk).

```
repeat k times:
    j = 0
    while j + 1 < len(num) and num[j] <= num[j + 1]: j += 1
    remove num[j]
return num.lstrip("0") or "0"
```

## Bottleneck

- Each removal restarts at 0 and rechecks the same rising prefix.
- Only the end of what you've kept is compared with the new digit. That is a stack.

## Optimised

```
stack = []
for d in num:
    while k and stack and stack[-1] > d:
        stack.pop(); k -= 1
    stack.append(d)
if k: stack = stack[:-k]
return "".join(stack).lstrip("0") or "0"
```

## Invariant

- The kept digits never go down from left to right.
- A bigger digit in front of a smaller one always gives a bigger number, whatever follows. So popping it is safe.

## Time and space

- Time O(n). Each digit is pushed and popped once.
- Space O(n).

## Variation

Q: What if you want the largest number instead?

A: Flip the comparison. Pop while `stack[-1] < d`.
