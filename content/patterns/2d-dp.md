---
id: 2d-dp
name: 2-D dynamic programming
order: 14
problems: [62, 1143, 72, 416, 518]
---

## Core idea

- Same as 1-D DP, but two arguments change between calls. Two arguments make a grid.
- Write the recursion, then list every argument that changes. Each one goes in the state.
- Arguments that never change, like nums or the two strings, don't.

## Recognise it

- two strings, and the question is about lining them up: longest common, fewest edits
- a grid crossed with only right and down moves: count the paths or find the cheapest
- take or skip each item once, toward a target: "split into two equal halves", "reach exactly"
- sizes that fit a grid: two strings up to 1000 each is 10^6 cells

### Questions to ask

1. Which arguments change between calls? That's the state.
2. 1-D or 2-D? Coin Change has one, because every coin is always allowed. Partition has two, because which numbers are left depends on how far you've got.
3. What does row 0 mean? Ask it like any base case. It isn't always 0.
4. Each item once, or any number of times? That decides which row the "take" reads.

### Looks right but isn't

- Every item usable any number of times and order counts (377). One number is enough, so it's 1-D.
- Two strings, but you only need to know if one is a subsequence of the other (392). Two pointers, O(n).

## Brute force it beats

- A recursion with two changing arguments. Each step branches, so O(2^(m + n)).
- Bottleneck: only (m + 1)(n + 1) different pairs exist. Cache them, or fill the grid.

## Templates

| | grid paths | two strings | take or skip |
| -- | -- | -- | -- |
| dp[i][j] | ways or cost to reach cell (i, j) | answer for s[:i] and t[:j] | can or how many with items[:i] making j |
| reads | up, left | up-left, up, left | the row above, and this row if reusable |
| row 0 | 1 path along the top | the empty prefix | no items, only j = 0 works |

### Grid paths

Cue: right and down moves only, count paths or find the cheapest.

- The last move into a cell is from above or from the left.
- Top row and left column have one path each.

```python
dp = [[1] * n for _ in range(m)]
for r in range(1, m):
    for c in range(1, n):
        dp[r][c] = dp[r - 1][c] + dp[r][c - 1]
return dp[m - 1][n - 1]
```

### Two strings

Cue: "longest common subsequence", "edit distance", "is s an interleaving".

- i is a length, so the table is (m + 1) x (n + 1). Compare `s[i - 1]`, the last letter of the prefix.
- Last letters match: up-left.
- Otherwise: try dropping each last letter.

```python
dp = [[0] * (n + 1) for _ in range(m + 1)]
for i in range(1, m + 1):
    for j in range(1, n + 1):
        if a[i - 1] == b[j - 1]:
            dp[i][j] = dp[i - 1][j - 1] + 1
        else:
            dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
return dp[m][n]
```

Edit distance: row 0 is j inserts, column 0 is i deletes. A mismatch is 1 + min of replace (up-left), delete (up), insert (left).

### Take or skip

Cue: "split into two equal sums", "reach exactly", "how many combinations".

- Skip reads the row above.
- Take once reads the row above at `t - x`. Take again reads this row.
- One row: once means t goes down, so `dp[t - x]` is still the old row. Again means t goes up.

```python
dp = [False] * (target + 1); dp[0] = True
for x in nums:
    for t in range(target, x - 1, -1):     # down, each x once
        dp[t] = dp[t] or dp[t - x]
```

Coin loop outside counts combinations. Amount loop outside counts orderings.

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 62 | Unique Paths | grid paths | up + left, top row and left column are 1 |
| 1143 | Longest Common Subsequence | two strings | match: up-left + 1, else max(up, left) |
| 72 | Edit Distance | two strings | mismatch: 1 + min(replace, delete, insert), row 0 = j |
| 416 | Partition Equal Subset Sum | take or skip | half the total, each number once, t goes down |
| 518 | Coin Change II | take or skip | coins outside, amount inside, dp[0] = 1 |

## Cost

- Time O(m · n), one cell each.
- Space O(m · n), or O(n) when each row only reads the row above.
