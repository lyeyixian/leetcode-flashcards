---
id: two-pointers
name: Two pointers
order: 1
problems: [167, 11, 125, 15]
---

## Core idea

- Two indices walk over one sequence.
- Each step, move the pointer that can't be part of a better answer.
- The question you must answer out loud: why is it safe to move that pointer?

## Recognise it

- "sorted array", "pair that sums to", "triplet"
- "palindrome", "reads the same both ways"
- "container", "area between two lines"
- "in place", "move all X to the end", "remove duplicates"

### Questions to ask

1. Am I looking for a pair, or something I can reduce to a pair? 3Sum is a pair once one number is fixed.
2. Does moving a pointer push the thing I track one way only?
    - Moving `left` only makes the sum bigger, moving `right` only makes it smaller.
    - It doesn't have to be sorted. Width only shrinks in 11, and that's enough.
3. Can I prove every other pair with that pointer is no better? Then I can drop it.

### Looks right but isn't

- Pair sum on unsorted input that needs the original indices (Two Sum, 1). Sorting loses the indices. Use a hash map.
- Subarray with a running condition (209, 3). Same direction pointers, but the window keeps state. That's sliding window.

## Brute force it beats

- Every pair, so O(n²).
- Bottleneck: most pairs can be ruled out without looking. One comparison tells you a whole row of pairs is useless.

## Templates

| | opposite ends | same direction |
| -- | -- | -- |
| start | `l = 0`, `r = n - 1` | both at 0 |
| each step | look at the pair, move one pointer inward | fast pointer reads, slow pointer marks where to write |
| needs | sorted input, or a one way argument | a rule for which elements to keep |
| stops | when they meet | when the fast one reaches the end |

### Opposite ends

Cue: "sorted", "pair that sums to", "palindrome", "container".

- Start at both ends.
- Look at the pair. Record or return if it's the answer.
- Move the one pointer that can't be in a better pair.

```python
l, r = 0, n - 1
while l < r:
    if found(a[l], a[r]):
        return answer
    if need_bigger:
        l += 1
    else:
        r -= 1
```

### Same direction

Cue: "in place", "move zeroes", "remove duplicates from sorted array".

- Fast pointer scans every element.
- Slow pointer is the next slot to write a kept element.

```python
write = 0
for read in range(n):
    if keep(a[read]):
        a[write] = a[read]
        write += 1
return write
```

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 167 | Two Sum II | opposite ends | sum too small, move left. Too big, move right |
| 11 | Container With Most Water | opposite ends | move the shorter line |
| 125 | Valid Palindrome | opposite ends | skip non letters, compare lowercase |
| 15 | 3Sum | opposite ends | sort, fix one, two sum the rest, skip repeats |

## Cost

- Time O(n). Each pointer moves at most n times. O(n log n) if you sort first.
- Space O(1), apart from the sort.
