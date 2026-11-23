---
number: 207
title: Course Schedule
difficulty: Medium
url: https://leetcode.com/problems/course-schedule/
pattern: graphs
template: topological sort
source: attempted
ask: "[a, b] means take b before a. Return true if you can take every course."
example: "n = 2, [[1,0]] gives true. [[1,0],[0,1]] gives false"
constraints: "1 <= numCourses <= 2000, 0 <= len(prerequisites) <= 5000, pairs unique"
---

## Pattern

- Graphs, topological sort template, Kahn's algorithm.
- Key: in-degree 0 queue, taken == n.

## Clarify

- [a, b] means b comes first?
- Courses with no prerequisites at all?
- This is really "is there a cycle"?

## Brute force

About O(V · (V + E)).

```
for each course:
    follow its prerequisites down to the end
    if you come back to a course on the current path: return False
return True
```

## Bottleneck

- Long chains are walked again from every course.
- Counting how many prerequisites each course still waits for handles each course once.

## Optimised

```
for a, b in prereqs: after[b].append(a); indeg[a] += 1
q = deque(c for c in range(n) if indeg[c] == 0), taken = 0
while q:
    c = q.popleft(); taken += 1
    for nxt in after[c]:
        indeg[nxt] -= 1
        if indeg[nxt] == 0: q.append(nxt)
return taken == n
```

- Other way: DFS with three states, not seen, on the path, done. Meeting one on the path is a cycle.

## Invariant

- `indeg[c]` is how many of c's prerequisites are not taken yet.
- A course on a cycle always waits on another in the cycle, so it never reaches 0.

## Time and space

- Time O(V + E).
- Space O(V + E).

## Variation

Q: What if you need an order to take them in (210)?

A: Append each course as you pop it. Return the list if it has all n, else [].
