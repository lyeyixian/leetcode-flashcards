---
number: 763
title: Partition Labels
difficulty: Medium
url: https://leetcode.com/problems/partition-labels/
pattern: greedy
template: cut at the farthest last index
source: attempted
ask: Cut s into as many parts as you can so each letter appears in one part only. Return the part sizes.
example: "\"ababcbacadefegdehijhklij\" gives [9,7,8]"
constraints: "1 <= len(s) <= 500, lowercase a to z"
---

## Pattern

- Greedy, cut at the farthest last index template.
- Key: cut when i == the farthest last index.

## Clarify

- Lowercase a to z only?
- Sizes left to right?

## Brute force

O(n²).

```
for each gap i:
    if no letter in s[:i] appears in s[i:]: cut here
```

## Bottleneck

- The scan is asking where each letter last appears. Store that once.
- A cut's being allowed depends only on s, so one cut never blocks another. Take every allowed cut.

## Optimised

```
last = {c: i for i, c in enumerate(s)}
start = end = 0
for i, c in enumerate(s):
    end = max(end, last[c])
    if i == end: res.append(i - start + 1); start = i + 1
return res
```

## Invariant

- `end` is the farthest last index of any letter in the current part. Before i reaches it, some letter shows up again later.

## Time and space

- Time O(n).
- Space O(26).

## Variation

Q: Why not cut when the current letter ends, `last[c] == i`?

A: On "abab" that cuts after the second a, but b still shows up later.
