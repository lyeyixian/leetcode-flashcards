---
number: 79
title: Word Search
difficulty: Medium
url: https://leetcode.com/problems/word-search/
pattern: backtracking
template: grid paths
source: attempted
ask: Return true if word can be spelled by a path of up, down, left or right cells. A path can't reuse a cell.
example: "board = [[A,B,C,E],[S,F,C,S],[A,D,E,E]], word = \"ABCCED\" gives true, \"ABCB\" gives false"
constraints: "1 <= rows, cols <= 6, 1 <= len(word) <= 15, letters only"
---

## Pattern

- Backtracking, grid paths template.
- Key: mark the cell, recurse four ways, restore.

## Clarify

- No diagonals?
- Can a cell be used twice in one path? No.
- Case sensitive?

## Brute force

- Same search with no early stop, trying all paths of length len(word) from every cell. O(r · c · 4^L).

## Bottleneck

- Most paths go wrong at the first letter or two. Stop there.
- The cell you came from is marked, so each step has at most 3 ways.

## Optimised

```
def bt(r, c, k):
    if k == len(word): return True
    if out of bounds or board[r][c] != word[k]: return False
    board[r][c] = "#"
    found = any(bt(r + dr, c + dc, k + 1) for dr, dc in four directions)
    board[r][c] = word[k]
    return found
return any(bt(r, c, 0) for every cell)
```

## Invariant

- While `bt` runs, the cells on the current path hold "#". Every other cell holds its letter.

## Time and space

- Time O(r · c · 3^L), L = len(word).
- Space O(L) for the call stack.

## Variation

Q: What if you have a list of words to find (212)?

A: Running this once per word repeats the same walks. Put the words in a trie and walk the board once.
