---
id: sliding-window
name: Sliding window
order: 3
problems: [209, 3, 424, 567]
---

## Core idea

- Two pointers, `left` and `right`, both only move right.
- Grow the window with `right`, keep track of one condition.
- Once `left` can't start a better window, drop it for good.
- Always record when the window is valid.

## Recognise it

- "contiguous subarray" or "substring"
- "longest", "shortest", "at most k", "at least k"
- "of length k" or "a permutation of s1" means the size is fixed

### Questions to ask

1. Is the answer contiguous?
2. Is there one condition to track, and is it cheap to update when one element comes in or goes out?
3. When the window grows, does the condition only move one way?
    - Yes: every longer window that keeps `left` is no better, so `left` can go.
    - Fixed size: skip this question, `left` is forced out anyway.

### Looks right but isn't

- Subarray sum with negative numbers (560). A bigger window can have a smaller sum, so question 3 fails. Use prefix sum + hash map.
- "Exactly k". Usually doesn't move one way. Count "at most k" minus "at most k - 1", or use prefix sums.
- Subsequence instead of substring. Not contiguous, so not a window. Think DP.

## Brute force it beats

- Every start, every end, so O(n²) windows.
- Bottleneck: each new start walks over elements the last start already saw. The window keeps that work instead of redoing it.

## Templates

| | longest window | shortest window |
| -- | -- | -- |
| growing never flips the window from | invalid back to valid | valid back to invalid |
| drop `left` when the window is | invalid | valid |
| every longer window from `left` is useless because | they're all invalid | they're all valid, but longer |
| record | after the shrink, when it's valid again | before the shrink, it's `left`'s best window |

### Longest window

Cue: "longest substring with no repeats", "longest with at most k changes".

- Add `right`.
- While invalid, remove `left`, then move `left`.
- Record.

```python
l = best = 0
for r, x in enumerate(s):
    add(x)
    while invalid():
        remove(s[l]); l += 1     # use s[l], then move l
    best = max(best, r - l + 1)  # valid here
```

### Shortest window

Cue: "minimum length", "smallest subarray such that".

- Add `right`.
- While valid, record first, then remove `left` and move `left`.

```python
l, best = 0, float("inf")
for r, x in enumerate(nums):
    add(x)
    while valid():
        best = min(best, r - l + 1)  # record first
        remove(nums[l]); l += 1      # then shrink
```

### Fixed size window

Cue: "of length k", "a permutation of s1", "every k consecutive".

- Add `right`.
- Once the window is longer than k, remove `s[r - k]`.
- Once the window is k long, check or record.

```python
for r, x in enumerate(s):
    add(x)
    if r >= k:
        remove(s[r - k])   # the one that just left
    if r >= k - 1:
        check()            # only once the first full window exists
```

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 209 | Minimum Size Subarray Sum | shortest window | valid when sum >= target |
| 3 | Longest Substring Without Repeating Characters | longest window | valid when s[r] isn't already in the window |
| 424 | Longest Repeating Character Replacement | longest window | valid when window length - top count <= k |
| 567 | Permutation in String | fixed size window | match when every count is 0 |

## Cost

- Time O(n), even with a `while` inside the `for`. `l` and `r` each move right at most n times.
- Space is what the window tracks. O(1) for a sum, O(alphabet) for counts.
