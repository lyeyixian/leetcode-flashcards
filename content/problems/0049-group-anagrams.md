---
number: 49
title: Group Anagrams
difficulty: Medium
url: https://leetcode.com/problems/group-anagrams/
pattern: arrays-and-hashing
template: group by key
source: attempted
ask: Group the strings that are anagrams of each other. Any order.
example: "[\"eat\",\"tea\",\"tan\",\"ate\",\"nat\",\"bat\"] gives [[\"bat\"],[\"nat\",\"tan\"],[\"ate\",\"eat\",\"tea\"]]"
constraints: "1 <= len(strs) <= 10^4, 0 <= len(strs[i]) <= 100, lowercase a to z"
---

## Pattern

- Arrays and hashing, group by key template.
- Key: tuple of 26 counts.

## Clarify

- Lowercase a to z only?
- Empty strings allowed? They group together.
- Any order of groups, and inside a group?

## Brute force

O(n² · m log m), m = longest string.

```
for each word w:
    for each group g:
        if sorted(w) == sorted(g[0]):
            g.append(w); break
    else:
        start a new group with w
```

## Bottleneck

- Each word is compared against every group.
- Anagrams share a key. Use that key in a hash map and each word finds its group in O(1).

## Optimised

```
groups = defaultdict(list)
for w in strs:
    count = [0] * 26
    for c in w: count[ord(c) - ord('a')] += 1
    groups[tuple(count)].append(w)
return list(groups.values())
```

- Middle step: key on `"".join(sorted(w))`. O(n · m log m).

## Invariant

- Two words are anagrams exactly when their count tuples are equal.

## Time and space

- Time O(n · m). Building the key is O(m + 26).
- Space O(n · m) to hold every word.

## Variation

Q: What if the strings can hold any Unicode character?

A: A fixed array of 26 won't fit. Use the sorted string, or a sorted tuple of `Counter` items, as the key.
