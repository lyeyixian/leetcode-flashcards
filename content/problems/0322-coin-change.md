---
number: 322
title: Coin Change
difficulty: Medium
url: https://leetcode.com/problems/coin-change/
pattern: 1d-dp
template: every choice from a list
source: attempted
ask: Return the fewest coins that add up to amount, using each coin as often as you like. -1 if impossible.
example: "coins = [1,2,5], amount = 11 gives 3. coins = [2], amount = 3 gives -1"
constraints: "1 <= len(coins) <= 12, 1 <= coins[i] <= 2^31 - 1, 0 <= amount <= 10^4"
---

## Pattern

- 1-D DP, every choice from a list template.
- Key: min(dp[a - c] + 1), dp[0] = 0, rest inf.

## Clarify

- Unlimited use of each coin?
- Amount 0 gives 0?
- Return -1 when nothing works?

## Brute force

Exponential.

```
def need(left):
    if left == 0: return 0
    if left < 0: return inf
    return min(need(left - c) + 1 for c in coins)
```

## Bottleneck

- `need(left)` only depends on left, and is asked many times.
- Greedy biggest first fails. Coins [1,3,4] for 6 gives 4+1+1, but 3+3 is fewer.

## Optimised

```
dp = [inf] * (amount + 1); dp[0] = 0
for a in range(1, amount + 1):
    for c in coins:
        if c <= a: dp[a] = min(dp[a], dp[a - c] + 1)
return dp[amount] if dp[amount] != inf else -1
```

## Invariant

- dp[a] is the fewest coins that make a. inf means it can't be made yet.
- The last coin is one of the coins, so try each.

## Time and space

- Time O(amount · len(coins)).
- Space O(amount).

## Variation

Q: What if you want the number of ways to make the amount (518)?

A: Join with + instead of min, and loop coins outside, amounts inside, so each mix is counted once.
