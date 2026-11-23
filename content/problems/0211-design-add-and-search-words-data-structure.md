---
number: 211
title: Design Add and Search Words Data Structure
difficulty: Medium
url: https://leetcode.com/problems/design-add-and-search-words-data-structure/
pattern: tries
template: wildcard DFS
source: attempted
ask: addWord(word) adds a word. search(word) returns true if some word matches, where "." matches any one letter.
example: "add bad, dad, mad. search(\"pad\") gives false, search(\".ad\") gives true, search(\"b..\") gives true"
constraints: "1 <= len(word) <= 25, at most 2 dots per search, at most 10^4 calls"
---

## Pattern

- Tries, wildcard DFS template.
- Key: a dot tries every child, True on the first match.

## Clarify

- A dot matches exactly one letter, not zero?
- How many dots per search? At most 2 here.

## Brute force

O(n · L) per search.

```
for w in words:
    if len(w) == len(q) and all(a == b or b == "." for a, b in zip(w, q)):
        return True
return False
```

## Bottleneck

- Every word is checked, even ones whose first letter is wrong.
- A trie follows one child per letter. Only a dot branches.

## Optimised

```
def dfs(node, i):
    if i == len(word): return node.is_end
    if word[i] == ".":
        return any(dfs(ch, i + 1) for ch in node.children.values())
    nxt = node.children.get(word[i])
    return nxt is not None and dfs(nxt, i + 1)
return dfs(root, 0)
```

## Invariant

- `dfs(node, i)` is True when `word[i:]` can be matched from node, ending on a word.

## Time and space

- Time O(L) with no dots. Each dot can multiply by 26.
- Space O(L) for the call stack.

## Variation

Q: What goes wrong if a dot returns the first child's answer?

A: Words "ab" and "ba", search(".a"). The a branch fails, and you return False before trying b.
