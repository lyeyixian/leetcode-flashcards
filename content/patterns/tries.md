---
id: tries
name: Tries
order: 15
problems: [208, 211, 648, 1268, 212]
---

## Core idea

- Store each prefix once, as a node. Letters live on the links, not in the nodes.
- A query walks one node per letter, so it's O(L), and the number of words never appears.
- Each node also has `is_end`, True only where an inserted word stops.

## Recognise it

- many prefix queries on one set of words: "starts with", autocomplete
- a wildcard that stands for one letter
- for each word in a text, the shortest or longest dictionary word it starts with
- many words to find in one grid, and most share starts
- numbers compared bit by bit from the top, like the largest XOR. The letters are 0 and 1.

### Questions to ask

1. Is every query a whole word? Then a hash set is simpler and just as fast.
2. Is a query a prefix, a wildcard, or a walk that stops partway? Then a trie.
3. What does a node store? Children and `is_end`, plus whatever the problem needs, like the whole word or the top 3 words.
4. What is one step of the walk, and where does it stop?

### Looks right but isn't

- "Does some word contain s in the middle". A trie answers prefixes only. For endings, insert each word reversed.
- One lookup per word, no prefixes. A set does it with less code.

## Brute force it beats

- Keep the words in a list and check each one. O(n · L) per query.
- Bottleneck: words that share a prefix walk it again for every word. A trie walks it once.

## Templates

| | plain walk | wildcard DFS | stop partway | grid with a trie |
| -- | -- | -- | -- | -- |
| children to follow | one | one, or all at a dot | one | the neighbour's letter |
| loop or recursion | loop | DFS | loop | DFS |
| stops at | the end of the word | the end, or every branch fails | the first end, or a fall-off | a node with no child for any neighbour |

### Plain walk

Cue: "implement a trie", insert, search, startsWith.

- Insert makes a child only when it's missing, then sets `is_end`.
- search must end on `is_end`. startsWith only must not fall off.

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

def insert(word):
    node = root
    for c in word:
        node = node.children.setdefault(c, TrieNode())
    node.is_end = True

def walk(s):
    node = root
    for c in s:
        node = node.children.get(c)
        if node is None: return None   # fell off
    return node
```

### Wildcard DFS

Cue: "." matches any one letter.

- A normal letter follows its one child.
- A dot tries every child. True on the first match, False only after all fail.

```python
def dfs(node, i):
    if i == len(word): return node.is_end
    c = word[i]
    if c == ".":
        return any(dfs(ch, i + 1) for ch in node.children.values())
    nxt = node.children.get(c)
    return nxt is not None and dfs(nxt, i + 1)
```

### Stop partway

Cue: "replace with the shortest root", "suggestions as the user types".

- Walk the word. Stop at the first `is_end`, which is the shortest match, no length compare needed.
- Or keep one node and move it one letter per keystroke. Once it falls off, stay off.

```python
node = root
for i, c in enumerate(word):
    node = node.children.get(c)
    if node is None: break
    if node.is_end: return word[:i + 1]
return word
```

### Grid with a trie

Cue: "find all these words in the grid".

- One trie of all words. One DFS from each cell, carrying the trie node.
- Step to a neighbour only if its letter is a child. That cuts every path that isn't the start of some word.
- Keep the whole word at its end node. Clear it once found, so it's added once.

```python
def dfs(r, c, node):
    ch = board[r][c]
    nxt = node.children.get(ch)
    if nxt is None: return
    if nxt.word: res.append(nxt.word); nxt.word = None
    board[r][c] = "#"
    for x, y in neighbours(r, c):
        if board[x][y] != "#": dfs(x, y, nxt)
    board[r][c] = ch
```

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 208 | Implement Trie (Prefix Tree) | plain walk | search checks is_end, startsWith doesn't |
| 211 | Design Add and Search Words Data Structure | wildcard DFS | a dot tries every child, True on the first match |
| 648 | Replace Words | stop partway | the first is_end on the way down is the shortest root |
| 1268 | Search Suggestions System | stop partway | each node keeps the first 3 sorted words, move one node per letter |
| 212 | Word Search II | grid with a trie | DFS carries the trie node, the word sits at its end node |

## Cost

- Time O(L) per insert or query.
- Space O(total letters) for the trie.
