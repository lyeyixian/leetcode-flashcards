---
number: 743
title: Network Delay Time
difficulty: Medium
url: https://leetcode.com/problems/network-delay-time/
pattern: advanced-graphs
template: Dijkstra
source: attempted
ask: times[i] = [u, v, w] is a directed edge taking w. A signal starts at k. Return how long until all n nodes receive it, or -1.
example: "times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2 gives 2"
constraints: "1 <= k <= n <= 100, 1 <= len(times) <= 6000, 0 <= w <= 100, no repeated (u, v)"
---

## Pattern

- Advanced graphs, Dijkstra template.
- Key: max of the final costs, -1 if not all reached.

## Clarify

- Directed edges?
- All weights >= 0?
- Nodes labelled 1 to n?

## Brute force

Exponential in the worst case.

```
best = {k: 0}
def dfs(u, d):
    for v, w in adj[u]:
        if d + w < best.get(v, inf):
            best[v] = d + w; dfs(v, d + w)
dfs(k, 0)
return max(best.values()) if len(best) == n else -1
```

## Bottleneck

- A node is entered again every time a cheaper path to it turns up.
- Pop the cheapest first. With w >= 0, a popped node can't get cheaper later.

## Optimised

```
adj[u].append((v, w)) for each edge
dist, h = {}, [(0, k)]
while h:
    d, u = heappop(h)
    if u in dist: continue
    dist[u] = d
    for v, w in adj[u]:
        if v not in dist: heappush(h, (d + w, v))
return max(dist.values()) if len(dist) == n else -1
```

## Invariant

- When u comes off the heap, `dist[u]` is the cheapest cost from k.
- Every push costs at least as much as the pop that made it, so pops never go down.

## Time and space

- Time O(E log E).
- Space O(V + E).

## Variation

Q: Why not BFS with a queue?

A: A queue gives nodes out by edge count, not cost. With 1 to 2 at 4, 1 to 3 at 1, 3 to 2 at 2, BFS says 4, but 1 to 3 to 2 is 3.
