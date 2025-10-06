# 416. Partition Equal Subset Sum

## Problem Statement
Given an integer array `nums`, return `true` if you can partition the array into two subsets such that the sum of the elements in both subsets is equal.

### Examples

**Example 1:**
```
Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

**Example 2:**
```
Input: nums = [1,2,3,5]
Output: false
Explanation: The array cannot be partitioned into equal sum subsets.
```

### Constraints:
- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`

## Solution
```java
class Solution {
    public static boolean canPartition(int[] nums) {
        int totalSum = 0;
        for (int num : nums) {
            totalSum += num;
        }

        // If the total sum is odd, it's not possible to partition it into two equal sum subsets
        if (totalSum % 2 != 0) {
            return false;
        }

        int target = totalSum / 2;
        boolean[] dp = new boolean[target + 1];
        dp[0] = true;  // There's always a way to make up sum 0 with an empty subset

        // Process each number in the array
        for (int num : nums) {
            // Go backwards to prevent using the same element more than once
            for (int j = target; j >= num; j--) {
                if (dp[j - num]) {
                    dp[j] = true;
                }
            }
        }

        return dp[target];
    }
}
```

## Approach
1. **Check Total Sum**:
   - Calculate the total sum of all elements in the array.
   - If the total sum is odd, it's impossible to partition the array into two equal subsets, so return `false`.

2. **Dynamic Programming Setup**:
   - The target sum for each subset is `totalSum / 2`.
   - Create a boolean array `dp` where `dp[i]` represents whether a subset with sum `i` can be formed.
   - Initialize `dp[0]` to `true` since a sum of 0 can always be achieved with an empty subset.

3. **DP Array Update**:
   - For each number in the array, update the `dp` array in reverse order to avoid reusing the same element multiple times.
   - For each number `num`, iterate from the target sum down to `num`.
   - If `dp[j - num]` is `true`, then set `dp[j]` to `true`.

4. **Result**:
   - The answer is `dp[target]`, which indicates whether a subset with sum equal to half of the total sum exists.

## Complexity
- **Time Complexity**: O(n * target)
  - Where `n` is the number of elements in the array and `target` is half of the total sum.
  - We iterate through each number and for each number, we potentially update the `dp` array from `target` down to the number's value.

- **Space Complexity**: O(target)
  - We use a boolean array of size `target + 1` to store the intermediate results.

## Edge Cases
- `nums = [1, 1]` → `true` (can be partitioned into [1] and [1])
- `nums = [1, 2, 5]` → `false` (cannot be partitioned into two equal sum subsets)
- `nums = [100]` → `false` (only one element, cannot be partitioned)
- `nums = [1, 5, 11, 5]` → `true` (can be partitioned into [1, 5, 5] and [11])

## Key Insights
- The problem reduces to finding a subset of numbers that sums up to half of the total sum of the array.
- If the total sum is odd, it's immediately impossible to partition the array into two equal sum subsets.
- The solution uses dynamic programming to efficiently check the possibility of forming the target sum using the given numbers.
- The reverse iteration in the inner loop ensures that each number is used at most once in the subset sum calculation.
- The approach is optimized to use O(target) space, making it efficient for large arrays with a relatively small total sum.