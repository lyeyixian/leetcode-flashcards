---
id: arrays-and-hashing
name: Arrays and hashing
order: 2
problems: [49, 128, 238, 560]
---

## Core idea

- Hashing: trade space for time. Store the past, so "have I seen X?" is O(1) instead of a scan.
    - Key: what the brute force inner loop searches for.
    - Value: what you need about it. An index, a count, or a list of members.
- Precomputation: store results by position, so a range sum or product is O(1) instead of recomputed.
- Look things up by value, use a hash map. Look things up by position, use an array.

## Recognise it

- "count", "seen before", "duplicate", "frequency"
- "pair that sums to" on unsorted input
- "group", "anagram", "same up to reordering"
- "how many subarrays sum to k"
- "O(n)" asked on unsorted input, where the easy answer is to sort
- "without division", "product of all the others"

### Questions to ask

1. Write the brute force. What does the inner loop search the past for?
2. Is it an exact match? A hash map can't answer "smaller than" or "closest".
3. Does the brute force recompute the same range sum or product? Can a range be undone, like a sum by subtraction? Then precompute it.

### Looks right but isn't

- Pair sum on sorted input with O(1) space (167). Use two pointers.
- Subarray sum with only positive numbers (209). A sliding window does it in O(1) space.
- "Closest" or "kth largest". A hash map has no order. Think sort, heap or binary search.

## Brute force it beats

- For each element, scan everything before it. O(n²).
- Bottleneck: the inner scan repeats the same search. Keep the past in a map keyed on what you search for.

## Templates

| | value to index or count | group by key | prefix sum count | set with run start | prefix and suffix |
| -- | -- | -- | -- | -- | -- |
| key | the value | a canonical form | prefix sum so far | the value | position |
| value | index or count | list of members | how many times | none | product or sum |
| order | look up, then record | record | look up, then record | build the set first | two passes |

### Value to index or count

Cue: "pair that sums to", "seen before", "duplicate", "count of".

- For each x, ask the map about what x needs.
- Then record x, so x can't pair with itself.

```python
seen = {}                          # value -> index
for i, x in enumerate(nums):
    if target - x in seen:
        return [seen[target - x], i]
    seen[x] = i                    # after the check
```

### Group by key

Cue: "group", "anagram", "same letters".

- Turn each item into a key that equal items share.
- Append the item to that key's list.

```python
groups = defaultdict(list)
for w in words:
    key = tuple(sorted(w))         # or a tuple of 26 counts
    groups[key].append(w)
return list(groups.values())
```

### Prefix sum count

Cue: "how many subarrays sum to k", "divisible by k", any subarray sum with negative numbers.

- sum(i+1..j) = prefix(j) - prefix(i). You want prefix(i) = prefix(j) - k.
- Seed the map with the empty prefix.
- Look up first, then record.

```python
seen = defaultdict(int)
seen[0] = 1                        # the empty prefix
acc = count = 0
for x in nums:
    acc += x
    count += seen[acc - k]         # look up first
    seen[acc] += 1                 # then record
```

### Set with run start

Cue: "consecutive", "longest streak", O(n) on unsorted input.

- Put everything in a set.
- Only walk up from x when x - 1 isn't there. Then each run is walked once.

```python
s = set(nums)
for x in s:                        # the set, not nums
    if x - 1 not in s:             # x starts a run
        end = x
        while end + 1 in s:
            end += 1
        best = max(best, end - x + 1)
```

### Prefix and suffix

Cue: "product of all except self", "without division", "range sum".

- One pass left to right, one pass right to left.
- Each pass keeps a running total for one side.

```python
out = [1] * n
p = 1
for i in range(n):                 # left side
    out[i] = p; p *= nums[i]       # use p, then update
p = 1
for i in range(n - 1, -1, -1):     # right side
    out[i] *= p; p *= nums[i]
```

## Problems

| # | Problem | Template | Key |
| -- | -- | -- | -- |
| 49 | Group Anagrams | group by key | tuple of 26 counts |
| 128 | Longest Consecutive Sequence | set with run start | only walk up from x when x - 1 is missing |
| 238 | Product of Array Except Self | prefix and suffix | left product times right product |
| 560 | Subarray Sum Equals K | prefix sum count | count prefix(j) - k, seed {0: 1} |

## Cost

- Time O(n). Each map lookup is O(1) on average.
- Space O(n) for the map. Prefix and suffix is O(1) apart from the output.
