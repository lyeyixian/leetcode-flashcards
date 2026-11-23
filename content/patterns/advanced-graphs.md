---
id: advanced-graphs
name: Advanced graphs
order: 17
problems: [743]
---

## Core idea

- Edges have different costs, and you want the cheapest path.
- Dijkstra: a heap of (cost so far, node). Always pop the cheapest. The first pop of a node is final.
- Every edge the same cost is plain BFS. It's faster.

## Recognise it

- weighted edges, "cheapest", "shortest time", "least cost"
- "how long until every node hears it": the largest cheapest cost from one start
- a path costs its worst step, "minimise the maximum": Dijkstra with max in place of +
- join every node for the least total, no start or end: a minimum spanning tree
- "at most k stops", or a negative cost: Bellman-Ford

### Questions to ask

1. What is a node, and what does an edge cost?
2. Are all costs 0 or more? Does a path's cost never go down as it gets longer? Then Dijkstra.
3. What does `dist[x]` hold when x comes off the heap, and why can nothing later lower it?
4. Is there a limit on the number of edges? Then Dijkstra breaks.

### Looks right but isn't

- Every edge costs the same. Plain BFS counts edges and is O(V + E).
- "At most k stops" (787). The cheapest way into a city may use up stops you need later. Bellman-Ford with k + 1 rounds.
- A negative edge. A push can be cheaper than the pop that made it, so a popped node isn't final.

## Brute force it beats

- DFS that re-enters a node whenever a cheaper path turns up. Right, but exponential in the worst case.
- Bottleneck: entering the same node again for every cheaper path. Pop the cheapest first and each node is final once.

## Templates

| | Dijkstra | Prim | Bellman-Ford |
| -- | -- | -- | -- |
| answers | cheapest path from one start | cheapest edges that join every node | cheapest path with at most k edges |
| heap or loop holds | (cost of the whole path, node) | (cost of one edge, node) | a dist list, rebuilt each round |
| breaks when | a negative edge, or a stop limit | never, for undirected | slow, O(V · E) |

### Dijkstra

Cue: weighted edges, cheapest path, all costs >= 0.

- Mark a node done when it's popped, not pushed.
- Skip a stale entry for a node that's already done.

```python
dist = {}
h = [(0, start)]
while h:
    d, u = heapq.heappop(h)
    if u in dist: continue
    dist[u] = d
    for v, w in adj[u]:
        if v not in dist:
            heapq.heappush(h, (d + w, v))
```

### Prim

Cue: "connect all points for the least total".

- Grow one tree. Push every edge out of a node when it joins.
- Pop the cheapest edge to a node not in the tree yet.

```python
seen, h, total = set(), [(0, 0)], 0
while len(seen) < n:
    w, u = heapq.heappop(h)
    if u in seen: continue
    seen.add(u); total += w
    for v, wv in adj[u]:
        if v not in seen: heapq.heappush(h, (wv, v))
```

### Bellman-Ford

Cue: "at most k stops", negative edges.

- After round i, `dist[x]` is the cheapest to x using at most i edges.
- Read from a copy, so one round adds at most one edge.

```python
dist = [inf] * n; dist[src] = 0
for _ in range(k + 1):
    nxt = dist[:]
    for u, v, w in edges:
        if dist[u] + w < nxt[v]: nxt[v] = dist[u] + w
    dist = nxt
```

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 743 | Network Delay Time | Dijkstra | max of the final costs, -1 if not all reached |

## Cost

- Dijkstra: time O(E log E), space O(V + E).
- Prim: same as Dijkstra.
- Bellman-Ford: time O(k · E), space O(V).
