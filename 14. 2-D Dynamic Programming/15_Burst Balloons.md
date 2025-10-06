# 312. Burst Balloons

## Problem Statement
You are given `n` balloons, indexed from `0` to `n - 1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.

If you burst the `ith` balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. After the burst, the `i-th` balloon's adjacent balloons become adjacent to each other.

Return the maximum coins you can collect by bursting the balloons wisely.

## Examples

### Example 1:
```
Input: nums = [3,1,5,8]
Output: 167
Explanation:
nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167
```

### Example 2:
```
Input: nums = [1,5]
Output: 10
Explanation:
nums = [1,5] --> [5] --> []
coins = 1*1*5 + 1*5*1 = 10
```

## Approach
We use dynamic programming to solve this problem:
1. Add 1's to both ends of the array to handle edge cases
2. Create a DP table where `dp[left][right]` represents the maximum coins obtained by bursting all balloons between `left` and `right` (inclusive)
3. For each possible subarray length from 1 to n:
   - For each possible starting index `left`:
     - Calculate the ending index `right`
     - For each position `i` in `[left, right]`, consider it as the last balloon to burst in this range
     - Calculate the coins for this scenario and update the DP table
4. The result will be in `dp[1][n]` (using 1-based indexing for the original array)

## Solution Code
```java
class Solution {
    public int maxCoins(int[] nums) {
        int n = nums.length;
        // Add 1's to both ends
        int[] extendedNums = new int[n + 2];
        extendedNums[0] = 1;
        extendedNums[n + 1] = 1;
        
        // Copy the original nums array into extendedNums
        for (int i = 1; i <= n; i++) {
            extendedNums[i] = nums[i - 1];
        }
        
        // DP table to store maximum coins
        // dp[left][right] = max coins from bursting balloons from left to right
        int[][] dp = new int[n + 2][n + 2];
        
        // Fill the DP table
        for (int length = 1; length <= n; length++) {  // Length of subarray
            for (int left = 1; left <= n - length + 1; left++) {
                int right = left + length - 1;
                
                // Try every position as the last balloon to burst in this range
                for (int i = left; i <= right; i++) {
                    // Coins from bursting balloon i last in this range
                    int coins = extendedNums[left - 1] * extendedNums[i] * extendedNums[right + 1];
                    // Add coins from left and right subproblems
                    coins += dp[left][i - 1] + dp[i + 1][right];
                    // Update the maximum coins for this range
                    dp[left][right] = Math.max(dp[left][right], coins);
                }
            }
        }
        
        // The result is the maximum coins obtained by bursting all balloons
        return dp[1][n];
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n³) - Three nested loops: one for subarray length, one for starting index, and one for the last balloon to burst
- **Space Complexity**: O(n²) - For the DP table

## Key Insights
1. **DP State Definition**: `dp[left][right]` represents the maximum coins obtained by bursting all balloons between `left` and `right`
2. **Last Balloon Strategy**: By considering each balloon as the last one to burst in a range, we can break down the problem into smaller subproblems
3. **Extended Array**: Adding 1's to both ends simplifies edge cases when bursting the first or last balloon
4. **Bottom-up DP**: We build solutions for all possible subarray lengths, starting from length 1 up to n

## Edge Cases
1. Empty array (should return 0)
2. Single balloon (return its value)
3. All balloons have the same value
4. Increasing or decreasing sequence of values
5. Large input size (tests efficiency)

## Follow-up
How would you modify the solution to also return the order in which to burst the balloons to achieve the maximum coins?