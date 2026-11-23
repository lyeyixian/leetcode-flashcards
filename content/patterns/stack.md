---
id: stack
name: Stack and monotonic stack
order: 4
problems: [20, 155, 739, 503, 402, 853]
---

## Core idea

- A stack holds what is still unfinished. Only the newest one can finish next.
    - Push: something new that has to wait.
    - Pop: the top is finished. Matched, answered, or thrown away.
- Monotonic stack: before pushing `x`, pop everything `x` beats. The stack stays sorted for free.
- Decide what the stack holds first. Openers (20), days still waiting for a warmer day (739), digits kept for now (402).

## Recognise it

- "next greater", "next smaller", "nearest to the left that is smaller"
- "how many days until", "first warmer day"
- brackets, nesting, undo
- "remove k digits to make it smallest", "remove duplicate letters"
- arbitrary input order but a physical order in the story, like positions on a road. Sort by it first.

### Questions to ask

1. What does the stack hold, and when is one of them finished?
2. When `x` pops `y`, why will nothing later need `y`? Only two good answers:
    - `y` is finished. Its answer is known.
    - `x` dominates `y`. `x` is closer to everything later and at least as good. You need both.
3. If neither is true, it isn't a stack problem.

### Looks right but isn't

- "The warmest day after me", not the first warmer one. That needs every later day. Use a suffix max.
- "First unique character". Says "first" but isn't about greater or smaller. Use a hash map count.
- Only a yes or no on "does anything on that side beat me". A running max from that side is enough. A stack that only ever reads its top is the hint (853).

## Brute force it beats

- For each element, scan forward until something beats it. O(n²).
- Bottleneck: the scan walks over the same elements again and again. The stack keeps only the ones still waiting.

## Templates

| | matching | resolve on pop | the stack is the answer |
| -- | -- | -- | -- |
| stack holds | openers waiting to close | indices waiting for an answer | the result so far |
| pop when | a closer arrives | `x` beats the top | the top is worse and you can still remove |
| at the end | must be empty | leftovers keep the default | clean up and return it |

### Matching

Cue: brackets, nesting, undo.

- Opener: push.
- Closer: the stack can't be empty, and the popped top must match.
- At the end, the stack must be empty.

```python
pair = {")": "(", "]": "[", "}": "{"}
stack = []
for c in s:
    if c not in pair:
        stack.append(c)
    elif not stack or stack.pop() != pair[c]:
        return False
return not stack
```

### Resolve on pop

Cue: "next greater", "next smaller", "how many days until".

- The stack holds indices still waiting.
- While `x` beats the top, pop `j` and record its answer.
- Then always push `i`.

```python
ans = [0] * n
stack = []                              # indices, not values
for i, x in enumerate(nums):
    while stack and nums[stack[-1]] < x:
        j = stack.pop()
        ans[j] = i - j                  # record as you pop
    stack.append(i)
```

| want | pop while | answer is |
| -- | -- | -- |
| next greater to the right | `nums[top] < x` | `i`, for each popped `j` |
| next smaller to the right | `nums[top] > x` | `i`, for each popped `j` |
| previous greater to the left | `nums[top] <= x` | `stack[-1]` after the while |
| previous smaller to the left | `nums[top] >= x` | `stack[-1]` after the while |

Strict or not matters when values repeat. "Warmer" means an equal day doesn't pop.

### The stack is the answer

Cue: "smallest number after removing k digits", "most competitive subsequence".

- While you can still remove and the top is worse than `d`, pop and spend one.
- Push `d`.
- After the loop, spend what's left from the end, then fix the format.

```python
stack = []
for d in num:
    while k and stack and stack[-1] > d:
        stack.pop(); k -= 1
    stack.append(d)
if k:
    stack = stack[:-k]                  # never went down, drop from the end
return "".join(stack).lstrip("0") or "0"
```

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 20 | Valid Parentheses | matching | closer must match the newest opener |
| 155 | Min Stack | matching | store the min so far with each value |
| 739 | Daily Temperatures | resolve on pop | pop days colder than today, answer i - j |
| 503 | Next Greater Element II | resolve on pop | second pass over nums, pop only |
| 402 | Remove K Digits | the stack is the answer | pop a bigger digit in front of a smaller one |
| 853 | Car Fleet | the stack is the answer | sort by position, a car that arrives no later joins the fleet ahead |

## Cost

- Time O(n), even with a `while` inside the `for`. Each element is pushed once and popped at most once.
- Space O(n) for the stack.
