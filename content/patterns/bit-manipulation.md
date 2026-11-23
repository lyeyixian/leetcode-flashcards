---
id: bit-manipulation
name: Bit manipulation
order: 16
problems: []
---

## Core idea

- Treat a number as a row of bits, and work on all of them at once with `&`, `|`, `^`, `<<`, `>>`.
- XOR is the one to remember. `x ^ x = 0` and `x ^ 0 = x`, so pairs cancel and order doesn't matter.
- No attempted problems here yet. This page will fill in once you've done some.

## Recognise it

- "every element appears twice except one"
- "count the 1 bits", "is it a power of two"
- "without using + or -"
- O(1) extra space where a hash set would be the easy answer
- a set of up to about 20 items stored as one number, a bitmask

### Questions to ask

1. Do pairs cancel? Then XOR everything.
2. Is it about one bit at a time? Then shift and mask.
3. Is the input a small set of flags? Then a bitmask, and loop over `range(1 << n)`.

### Looks right but isn't

- Negative numbers in Python. Ints have no fixed width, so `>>` on a negative never reaches 0. Mask with `& 0xFFFFFFFF` to act like 32 bits.
- "Appears twice except one" with no space limit. A hash map count is fine and easier to explain. Lead with it, then offer XOR.

## Brute force it beats

- A hash map or set for counting. O(n) space.
- Bottleneck: the space. Bits pack the same information into one number.

## Templates

| | XOR cancels pairs | check and clear bits | bitmask subsets |
| -- | -- | -- | -- |
| cue | "every other one appears twice" | "count 1 bits", "power of two" | "all subsets" of a small set |
| key move | `ans ^= x` | `x & (x - 1)` clears the lowest 1 | `mask >> i & 1` |

### XOR cancels pairs

Cue: "every element appears twice except one", "missing number".

```python
ans = 0
for x in nums:
    ans ^= x
return ans
```

### Check and clear bits

Cue: "number of 1 bits", "is it a power of two".

- `x & 1` is the lowest bit. `x >> 1` drops it.
- `x & (x - 1)` clears the lowest 1. A power of two has exactly one 1.

```python
count = 0
while x:
    x &= x - 1
    count += 1
```

### Bitmask subsets

Cue: every subset of up to about 20 items.

- Bit i of `mask` says whether item i is in.

```python
for mask in range(1 << n):
    subset = [nums[i] for i in range(n) if mask >> i & 1]
```

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |

## Cost

- Time O(n), or O(bits) per number, which is 32.
- Space O(1).
