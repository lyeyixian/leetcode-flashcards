---
number: 72
title: Edit Distance
difficulty: Medium
url: https://leetcode.com/problems/edit-distance/
pattern: 2d-dp
template: two strings
source: attempted
ask: Return the fewest single-letter inserts, deletes or replaces to turn word1 into word2.
example: "\"horse\", \"ros\" gives 3. \"\", \"abc\" gives 3"
constraints: "0 <= len(word1), len(word2) <= 500, lowercase"
---

## Pattern

- 2-D DP, two strings template, three choices on a mismatch.
- Key: mismatch: 1 + min(replace, delete, insert), row 0 = j.

## Clarify

- All three edits cost 1?
- Either string can be empty?

## Brute force

O(3^(m + n)).

```
def ed(i, j):
    if i == 0: return j
    if j == 0: return i
    if word1[i - 1] == word2[j - 1]: return ed(i - 1, j - 1)
    return 1 + min(ed(i - 1, j - 1), ed(i - 1, j), ed(i, j - 1))
```

## Bottleneck

- Only (m + 1)(n + 1) different pairs. Cache or fill the grid.

## Optimised

```
dp[i][0] = i, dp[0][j] = j
for i in 1..m:
    for j in 1..n:
        if word1[i - 1] == word2[j - 1]: dp[i][j] = dp[i - 1][j - 1]
        else: dp[i][j] = 1 + min(dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1])
return dp[m][n]
```

## Invariant

- dp[i][j] is the fewest edits to turn `word1[:i]` into `word2[:j]`.
- Up-left is replace, up is delete word1's last letter, left is insert word2's last letter.

## Time and space

- Time O(m · n).
- Space O(m · n), or O(n) with one row.

## Variation

Q: Why isn't row 0 all zeros?

A: dp[0][j] turns "" into `word2[:j]`, which takes j inserts. Same for column 0 with deletes.
