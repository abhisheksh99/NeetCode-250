# 213. House Robber II

## Problem Statement
You are a professional robber planning to rob houses arranged in a circle. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses are broken into on the same night.

Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

### Examples

**Example 1:**
```
Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```

**Example 2:**
```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

### Constraints:
- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 1000`

## Solution
```java
class Solution {
    public int rob(int[] nums) {
        // Edge case: only one house
        if (nums.length == 1) {
            return nums[0];
        }
        
        // Case 1: Rob houses from 0 to n-2 (skip the last house)
        int rob1 = 0;
        int rob2 = 0;
        int max1 = 0;
        
        for (int i = 0; i < nums.length - 1; i++) {
            max1 = Math.max(rob1 + nums[i], rob2);
            rob1 = rob2;
            rob2 = max1;
        }
        
        // Case 2: Rob houses from 1 to n-1 (skip the first house)
        rob1 = 0;
        rob2 = 0;
        int max2 = 0;
        
        for (int i = 1; i < nums.length; i++) {
            max2 = Math.max(rob1 + nums[i], rob2);
            rob1 = rob2;
            rob2 = max2;
        }
        
        // Return the maximum of the two cases
        return Math.max(max1, max2);
    }
}
```

## Approach
1. **Problem Analysis**:
   - This is a variation of the House Robber problem where the houses are arranged in a circle.
   - The main difference is that we cannot rob both the first and last house since they are adjacent in the circle.
   - We need to solve the problem by considering two scenarios:
     1. Rob houses from index 0 to n-2 (excluding the last house)
     2. Rob houses from index 1 to n-1 (excluding the first house)
   - The final result is the maximum of these two scenarios.

2. **Dynamic Programming (Space Optimized)**:
   - We use the same approach as the original House Robber problem but apply it to two different subarrays.
   - For each scenario, we maintain two variables `rob1` and `rob2` to keep track of the maximum amount that can be robbed up to two houses back and one house back, respectively.
   - We iterate through each house, updating these variables based on whether we rob the current house or not.

## Complexity
- **Time Complexity**: O(n)
  - We make two passes through the array (each of length n-1), resulting in O(2n) which simplifies to O(n).
- **Space Complexity**: O(1)
  - We use a constant amount of extra space (four variables) regardless of the input size.

## Edge Cases
- `nums = [1]` → `1` (only one house)
- `nums = [2, 3, 2]` → `3` (circular arrangement prevents robbing both first and last)
- `nums = [1, 2, 3, 1]` → `4` (rob first and third houses)
- `nums = [1, 2, 3]` → `3` (rob the middle house)

## Key Insights
- The circular arrangement can be handled by breaking it into two linear problems.
- By solving the problem for two different subarrays (excluding one end or the other), we cover all possible valid combinations.
- The space-optimized DP approach from the original House Robber problem is reused here for efficiency.
- The solution efficiently computes the maximum amount by considering the circular constraint through two separate linear passes.