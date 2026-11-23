---
id: backtracking
name: Backtracking
order: 9
problems: [78, 46, 39, 90, 79]
---

## Core idea

- You need every answer, so build each one a choice at a time.
- Choose, recurse, un-choose.
- Each recursive call is one level of nested loops you couldn't write by hand, because the depth depends on the input.

## Recognise it

- "return all": subsets, combinations, permutations, every valid string or board
- place things under rules and list the ones that work, like N-Queens or sudoku
- a path in a grid that can't reuse a cell
- small inputs: n up to about 20 for subsets, 8 to 10 for permutations, a 6 x 6 board

### Questions to ask

1. What does `path` hold?
2. What does this call loop over?
3. When do I record? At every call, or only when the path is complete?
4. Can I stop a branch early? That's where the speed comes from.

### Looks right but isn't

- "How many ways" or "what's the best". Only a count or a max, not the list. The same subtrees repeat, so it's DP.
- Inputs of 10^5. Exponential won't fit. Look for greedy or DP.

## Brute force it beats

- One nested loop per position. You can't write them when the count depends on the input.
- Listing everything is as big as the output, so it isn't faster than brute force. It only wins by pruning branches early.

## Templates

| | pick from the right | any unused | running total | repeats in the input | grid paths |
| -- | -- | -- | -- | -- | -- |
| loop over | start to end | all of nums | start to end | start to end | 4 neighbours |
| recurse with | `i + 1` | nothing changes | `i` | `i + 1` | next cell, `k + 1` |
| record when | every call | path is full | remaining is 0 | every call | k == len(word) |
| extra | | `used` flags | sort, break when too big | sort, skip `i > start` repeats | mark cell, restore |

### Pick from the right

Cue: "subsets", "combinations of size k". Order inside an answer doesn't matter.

- Loop from start, recurse with `i + 1`. Choices only move right, so each set comes out once.
- Record at the top of every call.

```python
def bt(start):
    res.append(path[:])        # a copy, path keeps changing
    for i in range(start, len(nums)):
        path.append(nums[i])
        bt(i + 1)
        path.pop()
```

### Any unused

Cue: "permutations", "all orderings".

- Loop over all of nums. A start index can't go back left to pick an earlier one.
- Set `used[i]` before the call, clear it after.
- Record when the path holds n.

```python
def bt():
    if len(path) == len(nums):
        res.append(path[:]); return
    for i in range(len(nums)):
        if used[i]: continue
        used[i] = True; path.append(nums[i])
        bt()
        path.pop(); used[i] = False
```

### Running total

Cue: "combinations that add up to target", "the same number may be chosen many times".

- Pass `remaining` down.
- Recurse with `i`, so the same number can come again. Never go left.
- Sort, then break once a candidate is bigger than remaining.

```python
def bt(start, remaining):
    if remaining == 0:
        res.append(path[:]); return
    for i in range(start, len(c)):
        if c[i] > remaining: break     # sorted, the rest won't fit
        path.append(c[i])
        bt(i, remaining - c[i])
        path.pop()
```

### Repeats in the input

Cue: "may contain duplicates", "each different answer once".

- Sort, so equal values sit together.
- `i > start` and equal to the one before: skip. It would rebuild a branch this loop already made.
- One level deeper, the same value is the first pick in its loop, so it's allowed.

```python
def bt(start):
    res.append(path[:])
    for i in range(start, len(nums)):
        if i > start and nums[i] == nums[i - 1]: continue
        path.append(nums[i])
        bt(i + 1)
        path.pop()
```

### Grid paths

Cue: "spell the word through the grid", each cell once per path.

- The choices are the four neighbours.
- Check bounds and the letter, mark the cell, recurse, put it back.
- Return a bool. `or` stops at the first True.

```python
def bt(r, c, k):
    if k == len(word): return True
    if not (0 <= r < R and 0 <= c < C) or board[r][c] != word[k]:
        return False
    board[r][c] = "#"
    found = (bt(r+1, c, k+1) or bt(r-1, c, k+1) or
             bt(r, c+1, k+1) or bt(r, c-1, k+1))
    board[r][c] = word[k]
    return found
```

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 78 | Subsets | pick from the right | record at every call, recurse i + 1 |
| 46 | Permutations | any unused | used flags, record when full |
| 39 | Combination Sum | running total | recurse with i, break when too big |
| 90 | Subsets II | repeats in the input | sort, skip i > start repeats |
| 79 | Word Search | grid paths | mark the cell, recurse four ways, restore |

## Cost

- Time is the size of the tree times the copy. Subsets O(n · 2^n), permutations O(n · n!).
- Depth is what one level uses up. One number, one letter, or part of the target.
- Space O(depth) for the path and the call stack, not counting the output.
