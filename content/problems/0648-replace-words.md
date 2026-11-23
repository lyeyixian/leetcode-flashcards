---
number: 648
title: Replace Words
difficulty: Medium
url: https://leetcode.com/problems/replace-words/
pattern: tries
template: stop partway
source: attempted
ask: Replace every word in sentence that starts with a root by the shortest such root. Return the new sentence.
example: "roots [cat, bat, rat], \"the cattle was rattled by the battery\" gives \"the cat was rat by the bat\""
constraints: "1 <= len(dictionary) <= 1000, 1 <= len(root) <= 100, 1 <= len(sentence) <= 10^6"
---

## Pattern

- Tries, stop partway template.
- Key: the first is_end on the way down is the shortest root.

## Clarify

- Shortest root wins when several fit?
- Words with no root stay as they are?
- Single spaces, no leading or trailing ones?

## Brute force

O(words · roots · L).

```
for each word:
    best = word
    for root in dictionary:
        if word.startswith(root) and len(root) < len(best): best = root
```

## Bottleneck

- Each word checks every root, and roots sharing a start are read again.
- Walk the word down a trie of roots. Shorter roots come first on the way down.

## Optimised

```
insert every root
def shortest(word):
    node = root
    for i, c in enumerate(word):
        node = node.children.get(c)
        if node is None: return word
        if node.is_end: return word[:i + 1]
    return word
return " ".join(shortest(w) for w in sentence.split())
```

## Invariant

- The walk passes "c", "ca", "cat", "catt" in order. The first `is_end` is the shortest root that fits.

## Time and space

- Time O(total letters in roots + total letters in sentence).
- Space O(total letters in roots).

## Variation

Q: What if you want the longest root instead?

A: Keep walking after an end. Remember the last `is_end` you passed, and stop only when you fall off.
