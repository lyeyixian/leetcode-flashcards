---
id: graphs
name: Graphs
order: 10
problems: [200, 994, 133, 207, 417]
---

## Core idea

- Say what a node is and what its edges are.
- Start somewhere, maybe from many nodes at once.
- DFS or BFS to visit the nodes. Mark each node the first time you see it.

## Recognise it

- a grid where cells join their up, down, left and right neighbours
- a list of pairs that link things: prerequisites, friendships, roads, edges
- a node object handed to you, with a list of neighbours
- "how many groups", "can I reach", "fewest steps", "is there a cycle", "in what order"

### Questions to ask

1. What is a node, and what are its edges? A grid works out neighbours from r and c. A list of pairs becomes an adjacency list.
2. DFS or BFS? BFS when the answer is a number of steps or minutes. Otherwise DFS is shorter.
3. When do I mark a node? When it goes in, not when it comes out.
4. Is searching from every node repeating the same paths? Then search backwards from the targets.

### Looks right but isn't

- Word Search (79). Also a grid, but one cell can be on many paths, so the mark is put back. That's backtracking. A flood fill only asks "seen or not", so its mark stays.
- Weighted edges and "cheapest path". Plain BFS counts edges, not cost. Use Dijkstra.
- Cells joined by a rule you can't check locally, like "same row anywhere". Think union-find or a hash map.

## Brute force it beats

- A DFS or BFS that remembers nothing. It runs forever on a cycle, or redoes the same paths from every start.
- Bottleneck: what it does over and over. The memory you add to stop that is the answer.

## Templates

| | flood fill | multi-source BFS | DFS with a dict | topological sort | search backwards | union-find |
| -- | -- | -- | -- | -- | -- | -- |
| cue | "how many islands" | "minimum minutes" from many starts | copy a node graph | prerequisites, order | "which cells can reach X" | edges arrive one by one |
| memory | turn the cell off | mark on push, count left | old node to copy | in-degree counts | one seen set per target | parent array |

### Flood fill

Cue: "how many islands", "regions", "connected groups".

- Loop over every cell. Each unseen land cell starts a group. Count it, then flood it.
- The flood marks every cell it touches. Never put the mark back.

```python
def dfs(r, c):
    if not (0 <= r < R and 0 <= c < C) or grid[r][c] != "1":
        return
    grid[r][c] = "0"
    dfs(r+1, c); dfs(r-1, c); dfs(r, c+1); dfs(r, c-1)

for r in range(R):
    for c in range(C):
        if grid[r][c] == "1":
            count += 1; dfs(r, c)
```

### Multi-source BFS

Cue: "minimum minutes", "distance to the nearest", spreading from many starts.

- Put every start in the queue first, so they spread together.
- One pass of `range(len(q))` is one level, one minute.
- Mark on push. Count what's left, to know when to stop and whether to return -1.

```python
minutes = 0
while q and fresh:
    for _ in range(len(q)):
        r, c = q.popleft()
        for x, y in neighbours(r, c):
            if grid[x][y] == 1:
                grid[x][y] = 2; fresh -= 1
                q.append((x, y))
    minutes += 1
return -1 if fresh else minutes
```

### DFS with a dict

Cue: you get a node and follow `neighbors`, and need something per node, like a copy.

- The dict is the visited set.
- Put the copy in the dict before visiting the neighbours, so a cycle finds it.

```python
copies = {}
def dfs(old):
    if old in copies: return copies[old]
    new = Node(old.val)
    copies[old] = new            # before the loop
    for nb in old.neighbors:
        new.neighbors.append(dfs(nb))
    return new
```

### Topological sort

Cue: "prerequisites", "dependencies", "in what order", "can all be done".

- `[a, b]` means b first, so the edge is b to a.
- Queue every node with in-degree 0. Pop, count it, lower its neighbours' in-degrees, queue the ones that hit 0.
- A node on a cycle never reaches 0. So `taken == n` means no cycle.

```python
for a, b in prereqs:
    after[b].append(a); indeg[a] += 1
q = deque(c for c in range(n) if indeg[c] == 0)
while q:
    c = q.popleft(); taken += 1
    for nxt in after[c]:
        indeg[nxt] -= 1
        if indeg[nxt] == 0: q.append(nxt)
return taken == n
```

### Search backwards

Cue: "which cells can reach the edge", "can flow to both oceans".

- Start from the targets and walk the path backwards. Water flows down, so the backwards walk goes up, `>=`.
- One seen set per target, seeded from all of its edge cells.
- The answer is the cells in both sets.

```python
def dfs(r, c, seen):
    if (r, c) in seen: return
    seen.add((r, c))
    for x, y in neighbours(r, c):
        if heights[x][y] >= heights[r][c]:
            dfs(x, y, seen)
```

### Union-find

Cue: edges arrive one at a time, "are these already joined", "redundant edge".

- `parent[x]` leads to x's root. A root points to itself.
- Join roots, never nodes.
- An edge whose ends share a root closes a cycle.

```python
def find(x):
    while parent[x] != x:
        parent[x] = parent[parent[x]]
        x = parent[x]
    return x

ra, rb = find(a), find(b)
if ra == rb: return [a, b]
parent[ra] = rb
```

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 200 | Number of Islands | flood fill | count each unseen land cell, then sink its island |
| 994 | Rotting Oranges | multi-source BFS | all rotten in the queue at once, one level is one minute |
| 133 | Clone Graph | DFS with a dict | old to copy, store before the neighbour loop |
| 207 | Course Schedule | topological sort | in-degree 0 queue, taken == n |
| 417 | Pacific Atlantic Water Flow | search backwards | uphill from each ocean's edges, answer is both sets |

## Cost

- Time O(V + E). Each node is handled once, each edge followed once. A grid is O(r · c).
- Space O(V). A recursive DFS on a grid of all land goes O(r · c) deep.
