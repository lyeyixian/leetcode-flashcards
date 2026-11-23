---
number: 518
title: Coin Change II
difficulty: Medium
url: https://leetcode.com/problems/coin-change-ii/
pattern: 2d-dp
template: take or skip
source: attempted
ask: Return how many combinations of coins make amount. Each coin can be used any number of times. Order doesn't matter.
example: "amount = 5, coins = [1,2,5] gives 4. amount = 3, coins = [2] gives 0"
constraints: "1 <= len(coins) <= 300, 1 <= coins[i] <= 5000, distinct, 0 <= amount <= 5000"
---

## Pattern

- 2-D DP, take or skip template, a coin can be taken again.
- Key: coins outside, amount inside, dp[0] = 1.

## Clarify

- 1+2 and 2+1 are the same combination?
- Amount 0 gives 1, the empty combination?

## Brute force

O(2^(n + amount)).

```
def ways(i, a):       # coins[:i] make a
    if a == 0: return 1
    if i == 0 or a < 0: return 0
    return ways(i - 1, a) + ways(i, a - coins[i - 1])
```

## Bottleneck

- Only n · amount different pairs. Cache or fill the grid.

## Optimised

```
dp = [0] * (amount + 1); dp[0] = 1
for c in coins:
    for a in range(c, amount + 1):
        dp[a] += dp[a - c]
return dp[amount]
```

## Invariant

- After processing some coins, dp[a] is the combinations of those coins that make a.
- Coins outside: by the time you're on coin 2, coin 1 is settled, so each mix is counted once.
- a goes up, so `dp[a - c]` may already use c. That's what lets c repeat.

## Time and space

- Time O(n · amount).
- Space O(amount).

## Variation

Q: What if order matters, so 1+2 and 2+1 count apart (377)?

A: Put the amount loop outside and coins inside. Every coin gets tried as the last one.
