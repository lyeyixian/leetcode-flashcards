---
number: 208
title: Implement Trie (Prefix Tree)
difficulty: Medium
url: https://leetcode.com/problems/implement-trie-prefix-tree/
pattern: tries
template: plain walk
source: attempted
ask: Build a Trie with insert(word), search(word) for an exact word, and startsWith(prefix).
example: "insert(\"apple\"), search(\"apple\") gives true, search(\"app\") gives false, startsWith(\"app\") gives true"
constraints: "1 <= len(word), len(prefix) <= 2000, lowercase, at most 3 * 10^4 calls"
---

## Pattern

- Tries, plain walk template.
- Key: search checks is_end, startsWith doesn't.

## Clarify

- Lowercase a to z only?
- search is exact, startsWith is any prefix?

## Brute force

O(n · L) per query.

```
words = []
search(w): return w in words
startsWith(p): return any(x.startswith(p) for x in words)
```

## Bottleneck

- Words sharing a prefix walk it again, one word at a time.
- Store each prefix once as a node, and a query walks only its own letters.

## Optimised

```
insert(word):
    node = root
    for c in word: node = node.children.setdefault(c, TrieNode())
    node.is_end = True
walk(s):
    node = root
    for c in s:
        node = node.children.get(c)
        if node is None: return None
    return node
search(w): n = walk(w); return n is not None and n.is_end
startsWith(p): return walk(p) is not None
```

## Invariant

- Each node is the prefix spelled on the way down. `is_end` marks where an inserted word stops.

## Time and space

- Time O(L) per call.
- Space O(total letters inserted).

## Variation

Q: Why does search("app") fail after only inserting "apple"?

A: The walk lands on a node, because "apple" passes through it. But that node's `is_end` is False.
