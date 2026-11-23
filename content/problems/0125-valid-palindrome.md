---
number: 125
title: Valid Palindrome
difficulty: Easy
url: https://leetcode.com/problems/valid-palindrome/
pattern: two-pointers
template: opposite ends
source: attempted
ask: Ignore case and anything that isn't a letter or digit. Return true if s reads the same forward and backward.
example: "\"A man, a plan, a canal: Panama\" gives true. \" \" gives true"
constraints: "1 <= len(s) <= 2 * 10^5, printable ASCII"
---

## Pattern

- Two pointers, opposite ends template.
- Key: skip non letters, compare lowercase.

## Clarify

- Digits count, punctuation and spaces don't?
- Case doesn't matter?
- A string that is empty after cleaning is true?

## Brute force

O(n) time, O(n) space.

```
clean = [c.lower() for c in s if c.isalnum()]
return clean == clean[::-1]
```

## Bottleneck

- Time is already O(n). You must look at every character.
- The cleaned copy costs O(n) space. Skip bad characters in place instead.

## Optimised

```
l, r = 0, len(s) - 1
while l < r:
    if not s[l].isalnum(): l += 1; continue
    if not s[r].isalnum(): r -= 1; continue
    if s[l].lower() != s[r].lower(): return False
    l += 1; r -= 1
return True
```

## Invariant

- Everything outside `[l, r]` already matches its mirror.

## Time and space

- Time O(n).
- Space O(1).

## Variation

Q: What if you may delete at most one character?

A: On the first mismatch, check whether `s[l+1..r]` or `s[l..r-1]` is a palindrome. That's 680 Valid Palindrome II.
