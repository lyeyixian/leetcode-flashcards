---
id: binary-search
name: Binary search
order: 5
problems: [704, 153, 33, 875, 1011]
---

## Core idea

- Check the middle, throw away the half that can't hold the answer, repeat.
- Answer three things before coding:
    1. What am I searching? An index, or an answer I guess and check.
    2. What condition reads F F F T T T along it?
    3. Why can't the half I drop hold the answer?

## Recognise it

- "sorted array", or "must be O(log n)"
- "first", "smallest index such that", "insert position"
- "minimum speed, capacity or days such that it works", "minimise the largest"
    - the answer is a number you can bound, and one guess is cheap to check
    - values up to 10^9, so you can't try every answer
- sorted in pieces, like a rotated array
- one step inside a bigger solution, on something that only grows. Prefix sums of positive numbers (209), timestamps (981).

### Questions to ask

1. What am I searching, indices or answers?
2. Write `ok(mid)`. Write its F and T row on an example, including the last index. Does it flip only once?
3. If `ok(mid)` is false, is everything left of mid false too? If true, is everything right of it true too?

### Looks right but isn't

- Subarray sums with negative numbers (560). The prefix can go down, so F and T mix. Binary search gives wrong answers with no error. Use prefix count in a hash map.
- One lookup in an unsorted array. Sorting first is O(n log n). A set is O(n).
- "Exactly" conditions. `hours(k) == h` isn't F F F T T T. Search on `hours(k) <= h` instead.
- Rotated array with duplicates (154, 81). When `nums[mid] == nums[-1]` you can't tell which side mid is on, so the worst case is O(n).

## Brute force it beats

- Check every index or every possible answer, one at a time. O(n), or O(range) for answers.
- Bottleneck: one check per element. A sorted condition lets one check rule out half.

## Templates

| | exact match | first true | search the answer |
| -- | -- | -- | -- |
| searching | indices | indices | possible answers |
| loop | `lo <= hi` | `lo < hi` | `lo < hi` |
| on true | return mid | `hi = mid` | `hi = mid` |
| on false | `lo = mid + 1` or `hi = mid - 1` | `lo = mid + 1` | `lo = mid + 1` |
| ends with | -1 if missing | `lo`, the first true | `lo`, the smallest that works |

### Exact match

Cue: sorted, distinct values, "return the index or -1".

- `lo <= hi`, because a range of one still needs checking.
- Hit: return mid.
- Both moves skip mid, since mid was just checked.

```python
lo, hi = 0, len(nums) - 1
while lo <= hi:
    mid = (lo + hi) // 2
    if nums[mid] == target:
        return mid
    if nums[mid] < target:
        lo = mid + 1          # mid and everything left are too small
    else:
        hi = mid - 1          # mid and everything right are too big
return -1
```

### First true

Cue: "first index such that", "insert position", rotated arrays.

- Write `ok(mid)` first.
- Pick `hi`. If "none" is possible, `hi = n`. If an answer always exists, `hi = n - 1`.
- `ok(mid)`: `hi = mid`, mid might be the answer.
- Not ok: `lo = mid + 1`. The `+ 1` guarantees progress, since mid rounds down.

```python
lo, hi = 0, len(nums)        # hi = n means nothing is true
while lo < hi:
    mid = (lo + hi) // 2
    if ok(mid):
        hi = mid
    else:
        lo = mid + 1
return lo
```

| want | `ok(mid)` |
| -- | -- |
| first `a[i] >= x`, `bisect_left` | `a[mid] >= x` |
| first `a[i] > x`, `bisect_right` | `a[mid] > x` |
| last `a[i] <= x` | `a[mid] > x`, then subtract 1 |
| min of a rotated array (153) | `nums[mid] <= nums[-1]` |

### Search the answer

Cue: "minimum speed", "least capacity", "minimise the largest".

- First true, run over answers instead of indices.
- `lo` and `hi` are the smallest and largest answers that could work.
- Write `feasible(guess)` as its own few lines.
- Check: if a guess works, every bigger guess works too.

```python
lo, hi = smallest_possible, largest_possible
while lo < hi:
    mid = (lo + hi) // 2
    if feasible(mid):
        hi = mid              # works, try smaller
    else:
        lo = mid + 1          # fails, and so does anything smaller
return lo
```

Integer ceiling is `(p + k - 1) // k`.

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 704 | Binary Search | exact match | drop the half on the wrong side of target |
| 153 | Find Minimum in Rotated Sorted Array | first true | first index with nums[mid] <= nums[-1] |
| 33 | Search in Rotated Sorted Array | exact match | one half is sorted, is target in its range? |
| 875 | Koko Eating Bananas | search the answer | hours(k) <= h, k from 1 to max(piles) |
| 1011 | Capacity To Ship Packages Within Days | search the answer | days(cap) <= days, cap from max to sum |

## Cost

- Time O(log n) for indices. O(cost of one check · log(range)) for answers.
- Space O(1).
