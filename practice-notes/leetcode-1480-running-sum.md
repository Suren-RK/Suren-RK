# LeetCode 1480 — Running Sum of 1D Array

**Topic:** Arrays · Prefix sums  
**Difficulty:** Easy  
**Problem:** https://leetcode.com/problems/running-sum-of-1d-array/

## Idea

The running sum at index `i` is the sum of every element from index `0` through `i`.

Instead of recalculating each prefix from scratch, keep a cumulative total and update it as we scan the array once.

## Python solution

```python
class Solution:
    def runningSum(self, nums):
        total = 0
        result = []

        for num in nums:
            total += num
            result.append(total)

        return result
```

## Dry run

Input: `[1, 2, 3, 4]`

| Number | Cumulative total | Result so far |
|---:|---:|---|
| 1 | 1 | `[1]` |
| 2 | 3 | `[1, 3]` |
| 3 | 6 | `[1, 3, 6]` |
| 4 | 10 | `[1, 3, 6, 10]` |

Output: `[1, 3, 6, 10]`

## Complexity

- **Time:** `O(n)` — each number is processed once.
- **Space:** `O(n)` for the returned result.

## Quick checks

- `[1, 2, 3, 4] -> [1, 3, 6, 10]`
- `[1, 1, 1, 1] -> [1, 2, 3, 4]`
- `[3, 1, 2] -> [3, 4, 6]`
- `[5] -> [5]`

## Key takeaway

Prefix sums are useful when later problems ask for cumulative totals or repeated range-sum calculations. This simple pattern is a foundation for more advanced array problems.
