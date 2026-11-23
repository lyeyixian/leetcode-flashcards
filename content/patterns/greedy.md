---
id: greedy
name: Greedy
order: 12
problems: [53, 134, 55, 45, 763]
---

## Core idea

- Make a good choice now, and explain why you never need the other choices.
- Two ways to prove it:
    - Swap: take a best answer, swap its first choice for yours. Still a best answer, so yours is safe.
    - Discard: show that what you throw away can't be part of a better answer.

## Recognise it

- one pass, and at each index there's a choice: jump or not, start here or not, cut here or not
- "can you reach", "fewest jumps", "where to start", "most parts", "largest sum of a subarray"
- sort first, then one pass, like the intervals that keep the earliest end

### Questions to ask

1. What does the brute force try at each step? Every jump, every start, every cut.
2. What one number sums up everything I've passed? A running total, the farthest reach, a level's end.
3. Why can I skip the other choices? Say it in one or two lines.
    - If you can't, try to break it with 3 or 4 numbers.
    - If it breaks, it's DP.

### Looks right but isn't

- Coin change with "biggest coin first" (322). Coins [1, 3, 4], amount 6 gives 4 + 1 + 1, but 3 + 3 is fewer. No swap fixes it. DP.
- Jump Game II with "jump as far as you can" from each spot. [2,3,1,1,4] takes three jumps, not two. Use levels.
- The right choice now depends on choices you haven't made yet. That's DP.

## Brute force it beats

- Try every choice at every step. A decision tree, often exponential, or O(n²).
- Bottleneck: most branches can be proven no better. Keep one number and skip them.

## Templates

| | running best | farthest reach | jump levels | cut at the farthest last index | two passes |
| -- | -- | -- | -- | -- | -- |
| kept | a total since the last restart | the farthest index reachable | this level's left and right | the farthest last index in this part | a value from each side |
| changes | restart when it goes negative | fail when i passes it | next level is right + 1 to the farthest | cut when i reaches it | keep the max of both |

### Running best

Cue: "largest sum of a subarray", "which start lets the total never go below 0".

- Keep the best total ending here.
- When it goes negative, nothing before can help, so start fresh.
- Keep the best anywhere separately.

```python
ans, cur = float("-inf"), 0
for x in nums:
    cur = max(x, cur + x)      # extend, or start fresh
    ans = max(ans, cur)
```

### Farthest reach

Cue: "can you reach the end", jumps of up to some length.

- A shorter jump is always allowed, so the reachable spots are a solid block from 0 to `far`.
- If `i > far`, you can't stand on i, or anything right of it.

```python
far = 0
for i, x in enumerate(nums):
    if i > far: return False
    far = max(far, i + x)
return True
```

### Jump levels

Cue: "fewest jumps", each spot reaches a range of later spots.

- BFS by level, but each level is a solid range. Keep its two ends instead of a queue.
- Next left is right + 1. Next right is the farthest reach from this level.

```python
left = right = jumps = 0
while right < len(nums) - 1:
    far = max(i + nums[i] for i in range(left, right + 1))
    left, right = right + 1, far
    jumps += 1
return jumps
```

### Cut at the farthest last index

Cue: "split into as many parts as you can" where some items must stay together.

- Store each letter's last index first.
- Stretch the part's end to the farthest last index seen.
- When i reaches it, every letter so far is done. Cut.

```python
last = {c: i for i, c in enumerate(s)}
start = end = 0
for i, c in enumerate(s):
    end = max(end, last[c])
    if i == end:
        res.append(i - start + 1); start = i + 1
```

### Two passes

Cue: a rule about both neighbours, "more than the neighbour on either side".

- Left to right fixes the rule with the left neighbour.
- Right to left fixes it with the right, keeping the max so the first pass still holds.

```python
c = [1] * n
for i in range(1, n):
    if r[i] > r[i - 1]: c[i] = c[i - 1] + 1
for i in range(n - 2, -1, -1):
    if r[i] > r[i + 1]: c[i] = max(c[i], c[i + 1] + 1)
```

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 53 | Maximum Subarray | running best | max(x, cur + x), start ans at -inf |
| 134 | Gas Station | running best | tank below 0 at i, restart at i + 1. Total < 0 means -1 |
| 55 | Jump Game | farthest reach | fail when i > far |
| 45 | Jump Game II | jump levels | levels are solid ranges |
| 763 | Partition Labels | cut at the farthest last index | cut when i == the farthest last index |

## Cost

- Time O(n), or O(n log n) with a sort first.
- Space O(1) for the running number. O(alphabet) when you store last indexes.
