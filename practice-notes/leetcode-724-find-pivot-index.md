# LeetCode 724 — Find Pivot Index

**Topic:** Arrays · Prefix sums  
**Difficulty:** Easy  
**Problem:** [Find Pivot Index](https://leetcode.com/problems/find-pivot-index/)

## Key idea

A pivot index is an index where the sum of all numbers strictly to its left equals the sum of all numbers strictly to its right.

Instead of calculating both sides repeatedly, calculate the total sum once and maintain a running left sum.

At index `i`:

```text
rightSum = totalSum - leftSum - nums[i]
```

If `leftSum == rightSum`, return `i`. Otherwise, add the current value to `leftSum` and continue.

## Java solution

```java
class Solution {
    public int pivotIndex(int[] nums) {
        int totalSum = 0;
        for (int num : nums) {
            totalSum += num;
        }

        int leftSum = 0;

        for (int i = 0; i < nums.length; i++) {
            int rightSum = totalSum - leftSum - nums[i];

            if (leftSum == rightSum) {
                return i;
            }

            leftSum += nums[i];
        }

        return -1;
    }
}
```

## Dry run

Input: `[1, 7, 3, 6, 5, 6]`

Total sum = 28.

| Index | Value | Left sum | Right sum | Match? |
|---:|---:|---:|---:|---|
| 0 | 1 | 0 | 27 | No |
| 1 | 7 | 1 | 20 | No |
| 2 | 3 | 8 | 17 | No |
| 3 | 6 | 11 | 11 | Yes |

Output: `3`

## Complexity

- **Time:** O(n) — one pass for total sum and one pass to find the pivot.
- **Space:** O(1) — only a few variables are used.

## Edge cases to test

1. `[1, 7, 3, 6, 5, 6]` → `3`
2. `[1, 2, 3]` → `-1`
3. `[2, 1, -1]` → `0`

## Takeaway

For equal left and right sums, use the total-sum relationship rather than nested loops. This turns a straightforward O(n²) approach into an O(n) solution with O(1) extra space.
