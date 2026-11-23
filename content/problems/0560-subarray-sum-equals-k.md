---
number: 560
title: Subarray Sum Equals K
difficulty: Medium
url: https://leetcode.com/problems/subarray-sum-equals-k/
pattern: arrays-and-hashing
template: prefix sum count
source: attempted
ask: Count the contiguous subarrays that sum to k. Numbers can be negative.
example: "[1,1,1], k = 2 gives 2. [1,-1,0], k = 0 gives 3"
constraints: "1 <= n <= 2 * 10^4, -1000 <= nums[i] <= 1000, -10^7 <= k <= 10^7"
---

## Pattern

- Arrays and hashing, prefix sum count template.
- Key: count prefix(j) - k, seed {0: 1}.

## Clarify

- Can numbers be negative? Yes, so a sliding window won't work.
- Count, not list?
- Can k be 0?

## Brute force

O(n²) with a running sum per start.

```
for i in range(n):
    s = 0
    for j in range(i, n):
        s += nums[j]
        if s == k: count += 1
```

## Bottleneck

- sum(i+1..j) = prefix(j) - prefix(i). For each j, the inner loop is counting earlier prefixes equal to prefix(j) - k.
- Keep a map from prefix value to how many times you've seen it. The count is one lookup.

## Optimised

```
seen = {0: 1}, acc = 0, count = 0
for x in nums:
    acc += x
    count += seen.get(acc - k, 0)
    seen[acc] = seen.get(acc, 0) + 1
return count
```

## Invariant

- Before recording `acc`, `seen` holds every prefix that ends before this element. The `{0: 1}` stands for the empty prefix, so a subarray starting at 0 counts.

## Time and space

- Time O(n).
- Space O(n) for the map.

## Variation

Q: What if you want the longest subarray that sums to k?

A: Store the first index of each prefix instead of a count, and never overwrite it. Length is `j - first[acc - k]`. That's 325.
