---
number: 20
title: Valid Parentheses
difficulty: Easy
url: https://leetcode.com/problems/valid-parentheses/
pattern: stack
template: matching
source: attempted
ask: s holds only ()[]{}. Return true if every opener is closed by the same type, in the right order.
example: "\"{[]}\" gives true. \"([)]\" gives false"
constraints: "1 <= len(s) <= 10^4, only the six bracket characters"
---

## Pattern

- Stack, matching template.
- Key: closer must match the newest opener.

## Clarify

- Only the six bracket characters?
- Is the empty string valid?
- An unclosed opener at the end is invalid?

## Brute force

O(n²).

```
while "()" in s or "[]" in s or "{}" in s:
    remove that pair from s
return s == ""
```

## Bottleneck

- Each pass rescans from the start, past openers that are still unmatched.
- Only the newest unmatched opener can close next. A stack keeps exactly those.

## Optimised

```
pair = {")": "(", "]": "[", "}": "{"}
stack = []
for c in s:
    if c not in pair: stack.append(c)
    elif not stack or stack.pop() != pair[c]: return False
return not stack
```

## Invariant

- The stack holds the unmatched openers, newest on top.
- A mismatch can fail at once. The top has to close before anything older can.

## Time and space

- Time O(n).
- Space O(n).

## Variation

Q: What if there is only one bracket type?

A: You don't need a stack. Keep a counter, fail if it goes below 0, and check it ends at 0.
