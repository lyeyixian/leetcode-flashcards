---
id: 1d-dp
name: 1-D dynamic programming
order: 13
problems: [70, 198, 322, 139, 300]
---

## Core idea

- You need every possible answer, but you remember results so you don't redo them.
- Write the recursion first. Then turn it into `dp[i]`.
- Four lines before code:
    1. `dp[i]` means, in one sentence.
    2. `dp[i]` comes from: which cells, joined by what.
    3. Base case.
    4. Fill order and answer.

## Recognise it

- "how many ways", "fewest", "most", "longest", "can you" over one array or string
- a choice at every step, and greedy breaks on 3 or 4 numbers
- the backtracking tree asks the same call in two places. Draw two levels and look.
- n up to 10^4 with a small inner loop, or up to 2500 for O(n²)
- backtracking returns every answer. DP returns one number about them.

### Questions to ask

1. Which smaller question does the recursion keep asking? Name it, and that's `dp[i]`.
2. Does the answer from here depend only on where I am, not how I got here? If not, the state needs a second number. That's 2-D DP.
3. What joins the choices?

| question | join | start value |
| -- | -- | -- |
| fewest | min | inf |
| most | max | 0 or -inf |
| can it be done | or | False |
| how many ways | + | 0 |

4. Base case: usually "nothing left". 0 stairs is 1 way. Amount 0 is 0 coins. The empty string is True.

### Looks right but isn't

- Choices that can be proven safe, like Jump Game (55). DP works but is O(n²). Greedy is O(n).
- Each coin can be used once (0/1 knapsack). `i` alone isn't enough. That's 2-D DP.
- "Return every way", not how many. The output is exponential. That's backtracking.

## Brute force it beats

- A recursion that tries every choice. Exponential.
- Bottleneck: the same call with the same argument comes up again and again. Cache it, or fill a table.

## Templates

| | last two | every choice from a list | look back at every earlier index | two values per index |
| -- | -- | -- | -- | -- |
| reads | dp[i - 1], dp[i - 2] | dp[i - c] for each choice c | every dp[j], j < i | the last max and min |
| cue | steps of 1 or 2, skip a neighbour | coins, words, reusable | subsequence | a negative can flip best |
| space | O(1) | O(n) | O(n) | O(1) |

### Last two

Cue: each step looks back one or two places.

- Keep two variables.
- Assign both in one tuple, so both sides read the old values.

```python
prev = curr = 1             # ways to reach step 0 and 1
for _ in range(n - 1):
    prev, curr = curr, prev + curr
return curr
```

### Every choice from a list

Cue: the same list of choices at every step, each usable again. Coins, words, step sizes.

- The last choice is one of the list, so try each.
- Join with what the question asks for.

```python
dp = [float("inf")] * (amount + 1)
dp[0] = 0
for i in range(1, amount + 1):
    for c in coins:
        if c <= i:
            dp[i] = min(dp[i], dp[i - c] + 1)
return dp[amount] if dp[amount] != float("inf") else -1
```

### Look back at every earlier index

Cue: subsequence, the item before this one can be any earlier item.

- `dp[i]` means "ends at i", so you know the last item to compare.
- The answer can end anywhere, so `max(dp)`.

```python
dp = [1] * n
for i in range(n):
    for j in range(i):
        if nums[j] < nums[i]:
            dp[i] = max(dp[i], dp[j] + 1)
return max(dp)
```

### Two values per index

Cue: like Maximum Subarray, but one step can flip which value is best, like multiplying by a negative.

```python
best = hi = lo = nums[0]
for x in nums[1:]:
    hi, lo = max(x, hi * x, lo * x), min(x, hi * x, lo * x)
    best = max(best, hi)
```

### From recursion to table

1. The argument becomes the index i.
2. The base case becomes the cells filled first.
3. Each call `f(j)` becomes `dp[j]`.
4. A cell can only read cells already filled. `f(i)` reads i + 1, so fill right to left.
5. The first call, `f(0)`, is the answer.

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 70 | Climbing Stairs | last two | dp[i - 1] + dp[i - 2], dp[0] = 1 |
| 198 | House Robber | last two | max(skip, rob + two back) |
| 322 | Coin Change | every choice from a list | min(dp[a - c] + 1), dp[0] = 0, rest inf |
| 139 | Word Break | every choice from a list | dp[i] = s[i:] can be cut, or over words, dp[n] = True |
| 300 | Longest Increasing Subsequence | look back at every earlier index | longest ending at i, answer max(dp) |

## Cost

- Time is the number of states times the work per state.
- Space is the table. O(1) when a cell only reads the last one or two.
