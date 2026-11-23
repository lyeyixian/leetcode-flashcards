---
number: 1268
title: Search Suggestions System
difficulty: Medium
url: https://leetcode.com/problems/search-suggestions-system/
pattern: tries
template: stop partway
source: attempted
ask: After each letter of searchWord, suggest up to 3 products that start with what's typed, smallest first.
example: "products [mobile, mouse, moneypot, monitor, mousepad], \"mouse\" gives [mobile, moneypot, monitor] twice, then [mouse, mousepad] three times"
constraints: "1 <= len(products) <= 1000, total letters <= 2 * 10^4, unique, 1 <= len(searchWord) <= 1000"
---

## Pattern

- Tries, stop partway template. One node moves as the user types.
- Key: each node keeps the first 3 sorted words, move one node per letter.

## Clarify

- Smallest 3 in alphabetical order?
- Once nothing matches, every later answer is []?

## Brute force

O(m · n · L).

```
products.sort()
for k in 1..len(searchWord):
    prefix = searchWord[:k]
    res.append([p for p in products if p.startswith(prefix)][:3])
```

## Bottleneck

- Each new prefix is the old one plus a letter, but the scan starts over.
- Move one trie node one letter. Store the answer at each node.

## Optimised

```
for p in sorted(products):
    node = root
    for c in p:
        node = node.children.setdefault(c, TrieNode())
        if len(node.top) < 3: node.top.append(p)
node = root
for c in searchWord:
    node = node.children.get(c) if node else None
    res.append(node.top if node else [])
```

- Other way: sort, then `bisect_left` for each prefix and take the next 3 that match.

## Invariant

- Products go in sorted, so each node's `top` is the smallest 3 with that prefix.
- Once `node` is None, no longer prefix can match, so it stays None.

## Time and space

- Time O(n log n · L) to sort and build, O(m) to answer.
- Space O(total letters).

## Variation

Q: What goes wrong if you `break` on a fall-off instead of setting node to None?

A: node stays on the last good node, so later letters keep giving its words.
