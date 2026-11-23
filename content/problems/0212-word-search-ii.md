---
number: 212
title: Word Search II
difficulty: Hard
url: https://leetcode.com/problems/word-search-ii/
pattern: tries
template: grid with a trie
source: attempted
ask: Return every word from words that can be spelled on board by adjacent cells, no cell reused per path. Each word once.
example: "board [[o,a,a,n],[e,t,a,e],[i,h,k,r],[i,f,l,v]], words [oath, pea, eat, rain] gives [eat, oath]"
constraints: "1 <= rows, cols <= 12, 1 <= len(words) <= 3 * 10^4, 1 <= len(words[i]) <= 10, unique"
---

## Pattern

- Tries, grid with a trie template.
- Key: DFS carries the trie node, the word sits at its end node.

## Clarify

- Up, down, left, right only?
- Each found word once, any order?

## Brute force

O(W · r · c · 3^L).

```
for word in words:
    if exist(board, word): res.append(word)   # 79 Word Search
```

## Bottleneck

- Words with the same start walk the same cells again.
- One trie of all words, one DFS per cell. Stop as soon as the path isn't a prefix of any word.

## Optimised

```
build the trie, node.word = word at each end
def dfs(r, c, node):
    ch = board[r][c]
    nxt = node.children.get(ch)
    if nxt is None: return
    if nxt.word: res.append(nxt.word); nxt.word = None
    board[r][c] = "#"
    for each in-bounds neighbour (x, y): dfs(x, y, nxt)
    board[r][c] = ch
for every cell: dfs(r, c, root)
```

## Invariant

- The path so far spells the prefix that `node` stands for. Cells on the path hold "#".
- Clearing `word` once found means it's added once, even if four paths reach it.

## Time and space

- Time O(W · L) to build, O(r · c · 4 · 3^(L - 1)) for the DFS.
- Space O(W · L) for the trie.

## Variation

Q: How can you speed it up further?

A: Remove a trie node once it has no children and no word. Later paths stop sooner.
