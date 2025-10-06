# 494. Target Sum

## Problem Statement
You are given an integer array `nums` and an integer `target`.

You want to build an expression out of nums by adding one of the symbols `'+'` and `'-'` before each integer in `nums` and then concatenate all the integers.

- For example, if `nums = [2, 1]`, you can add `'+'` before `2` and `'-'` before `1` and concatenate them to build the expression `"+2-1"`.

Return the number of different expressions that you can build, which evaluates to `target`.

## Examples

### Example 1:
```
Input: nums = [1,1,1,1,1], target = 3
Output: 5
Explanation: There are 5 ways to assign symbols to make the sum of nums be target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```

### Example 2:
```
Input: nums = [1], target = 1
Output: 1
```

## Approach
This problem can be reduced to finding the number of subsets of `nums` that sum up to a target value `S1`, where `S1 = (sum + target) / 2`.

Key steps:
1. Calculate the total sum of `nums`
2. Check for edge cases where it's impossible to achieve the target
3. Calculate `S1` which represents the sum of elements to be positive
4. Use dynamic programming to count the number of subsets that sum to `S1`

## Solution Code
```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        
        // Check if it is possible to partition
        if ((target + sum) % 2 != 0 || sum < Math.abs(target)) return 0;
        
        int S1 = (target + sum) / 2;
        
        // Early check to prevent negative array size
        if (S1 < 0) return 0;
        
        int[] dp = new int[S1 + 1];
        dp[0] = 1;  // There's one way to sum up to zero: use no elements
        
        for (int num : nums) {
            for (int j = S1; j >= num; j--) {
                dp[j] += dp[j - num];
            }
        }
        
        return dp[S1];
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n × S1) - Where n is the number of elements in `nums` and S1 is the target sum
- **Space Complexity**: O(S1) - For the DP array

## Key Insights
1. **Problem Reduction**: The problem reduces to finding the number of subsets that sum to `(sum + target) / 2`
2. **Edge Cases Handling**: 
   - If `(sum + target)` is odd, it's impossible to partition
   - If `sum < abs(target)`, no solution exists
   - If `S1` is negative, no solution exists
3. **DP Initialization**: `dp[0] = 1` because there's one way to make sum 0 (using no elements)
4. **State Transition**: For each number, update the DP array backwards to avoid counting duplicates

## Edge Cases
1. Empty array (should return 0 for any target != 0)
2. Single element array (should return 1 if element equals target)
3. All elements are the same
4. Target is 0 (check for empty subset)
5. Large input size (tests efficiency)

## Follow-up
How would you modify the solution if we could also multiply numbers in the expression (in addition to adding and subtracting)?