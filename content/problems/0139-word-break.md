---
number: 139
title: Word Break
difficulty: Medium
url: https://leetcode.com/problems/word-break/
pattern: 1d-dp
template: every choice from a list
source: attempted
ask: Return true if s can be cut into a sequence of words from wordDict. Words can repeat.
example: "\"applepenapple\", [apple, pen] gives true. \"catsandog\", [cats, dog, sand, and, cat] gives false"
constraints: "1 <= len(s) <= 300, 1 <= len(wordDict) <= 1000, 1 <= len(word) <= 20, lowercase, distinct"
---

## Pattern

- 1-D DP, every choice from a list template. Coin Change with words for coins.
- Key: dp[i] = s[i:] can be cut, or over words, dp[n] = True.

## Clarify

- Can a word be used more than once?
- Lowercase only?

## Brute force

Exponential.

```
def cut(i):
    if i == n: return True
    return any(s.startswith(w, i) and cut(i + len(w)) for w in wordDict)
```

## Bottleneck

- What's left is always `s[i:]`, so the state is just i. The same i comes up from many cuts.
- Taking the first word that fits fails. "catsand" with "cat" leaves "sand".

## Optimised

```
dp = [False] * (n + 1); dp[n] = True
for i in range(n - 1, -1, -1):
    for w in wordDict:
        if s.startswith(w, i):
            dp[i] = dp[i] or dp[i + len(w)]
return dp[0]
```

## Invariant

- dp[i] is True when `s[i:]` can be cut into words. It reads bigger i, so fill right to left.

## Time and space

- Time O(n · len(wordDict) · len(word)).
- Space O(n).

## Variation

Q: What if you need every way to cut it (140)?

A: The output can be exponential, so it's backtracking. Use this dp to prune positions that can't finish.
