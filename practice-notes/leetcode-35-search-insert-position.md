# LeetCode 35 — Search Insert Position

**Topic:** Arrays · Binary search  
**Difficulty:** Easy  
**Problem:** [Search Insert Position](https://leetcode.com/problems/search-insert-position/)

## Goal

Given a sorted array of distinct integers and a target, return the target's index if it exists. Otherwise, return the index where it should be inserted to keep the array sorted.

## Key idea: lower bound

The answer is the first index whose value is greater than or equal to the target. If every value is smaller, the answer is `nums.length`.

Use a half-open search range `[left, right)`:

- If `nums[mid] < target`, the answer must be to the right, so set `left = mid + 1`.
- Otherwise, `mid` could be the answer, so set `right = mid`.

When `left == right`, that position is the insertion index.

## Java solution

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0;
        int right = nums.length;

        while (left < right) {
            int mid = left + (right - left) / 2;

            if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }

        return left;
    }
}
```

## Dry run

Input: `nums = [1, 3, 5, 6]`, `target = 2`

| left | right | mid | nums[mid] | Decision |
|---:|---:|---:|---:|---|
| 0 | 4 | 2 | 5 | 5 is not smaller; right = 2 |
| 0 | 2 | 1 | 3 | 3 is not smaller; right = 1 |
| 0 | 1 | 0 | 1 | 1 is smaller; left = 1 |

Now `left == right == 1`, so return **1**.

## Examples and edge cases

| nums | target | Answer | Reason |
|---|---:|---:|---|
| `[1, 3, 5, 6]` | 5 | 2 | Target exists |
| `[1, 3, 5, 6]` | 2 | 1 | Insert between 1 and 3 |
| `[1, 3, 5, 6]` | 7 | 4 | Insert at the end |
| `[1, 3, 5, 6]` | 0 | 0 | Insert at the beginning |
| `[]` | 4 | 0 | Empty array |

## Complexity

- **Time:** O(log n) — the search range is halved each iteration.
- **Space:** O(1) — only a few variables are used.

## Common mistake

Using `right = mid - 1` with this half-open interval can skip the correct insertion position. Here, when `nums[mid] >= target`, keep `mid` in the candidate range by setting `right = mid`.

## Takeaway

Binary search can find more than an existing element. By searching for the first value that is not smaller than the target, it also finds the correct insertion position in O(log n) time.
