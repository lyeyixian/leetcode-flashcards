---
number: 1143
title: Longest Common Subsequence
difficulty: Medium
url: https://leetcode.com/problems/longest-common-subsequence/
pattern: 2d-dp
template: two strings
source: attempted
ask: Return the length of the longest subsequence both text1 and text2 have.
example: "\"abcde\", \"ace\" gives 3. \"abc\", \"def\" gives 0"
constraints: "1 <= len(text1), len(text2) <= 1000, lowercase"
---

## Pattern

- 2-D DP, two strings template.
- Key: match: up-left + 1, else max(up, left).

## Clarify

- Subsequence, so skipping letters is fine?
- Return the length only?

## Brute force

O(2^(m + n)).

```
def lcs(i, j):        # text1[:i], text2[:j]
    if i == 0 or j == 0: return 0
    if text1[i - 1] == text2[j - 1]: return 1 + lcs(i - 1, j - 1)
    return max(lcs(i - 1, j), lcs(i, j - 1))
```

## Bottleneck

- Only (m + 1)(n + 1) different (i, j) pairs, but the recursion asks them many times.

## Optimised

```
dp = [[0] * (n + 1) for _ in range(m + 1)]
for i in range(1, m + 1):
    for j in range(1, n + 1):
        if text1[i - 1] == text2[j - 1]: dp[i][j] = dp[i - 1][j - 1] + 1
        else: dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
return dp[m][n]
```

## Invariant

- dp[i][j] is the answer for `text1[:i]` and `text2[:j]`.
- Last letters match: some longest one uses both, so up-left + 1. Differ: one of them isn't used, so drop each and take the max.

## Time and space

- Time O(m · n).
- Space O(m · n), or O(n) with one row and a saved up-left.

## Variation

Q: What if you need the fewest deletions to make the strings equal (583)?

A: Keep the LCS, delete the rest. `m + n - 2 · lcs`.
