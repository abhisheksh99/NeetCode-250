# 198. House Robber

## Problem Statement
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

### Examples

**Example 1:**
```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 2:**
```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9), and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

### Constraints:
- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 400`

## Solution
```java
class Solution {
    public int rob(int[] nums) {
        // rob1 represents the maximum amount that can be robbed up to the house two positions before the current one
        // rob2 represents the maximum amount that can be robbed up to the previous house
        int rob1 = 0;
        int rob2 = 0;
        
        // Iterate through each house
        for (int num : nums) {
            // The maximum amount at the current house is the maximum of:
            // 1. The amount from robbing the current house plus the amount from two houses back
            // 2. The amount from the previous house (not robbing the current house)
            int currentMax = Math.max(rob1 + num, rob2);
            
            // Update rob1 and rob2 for the next iteration
            rob1 = rob2;
            rob2 = currentMax;
        }
        
        // The final result is stored in rob2
        return rob2;
    }
}
```

## Approach
1. **Dynamic Programming (Space Optimized)**:
   - We use two variables `rob1` and `rob2` to keep track of the maximum amount that can be robbed up to two houses back and one house back, respectively.
   - For each house, we decide whether to rob it or not:
     - If we rob the current house, we add its value to the amount from two houses back (`rob1`).
     - If we don't rob it, we take the amount from the previous house (`rob2`).
   - We take the maximum of these two options as the new maximum up to the current house.
   - We then update `rob1` and `rob2` for the next iteration.

2. **Base Case**:
   - If there are no houses, the maximum amount is 0.
   - If there's only one house, we can only rob that house.
   - The solution handles these cases naturally with the initialization of `rob1` and `rob2` to 0.

## Complexity
- **Time Complexity**: O(n)
  - We make a single pass through the array of houses, performing constant time operations at each step.
- **Space Complexity**: O(1)
  - We use only a constant amount of extra space (two variables) regardless of the input size.

## Edge Cases
- `nums = [1]` → `1` (only one house)
- `nums = [2, 1, 1, 2]` → `4` (alternate houses)
- `nums = [100, 1, 1, 100]` → `200` (skip two houses)
- `nums = [0]` → `0` (no money in the only house)

## Key Insights
- The problem follows the optimal substructure property of dynamic programming, where the solution to the problem depends on solutions to smaller subproblems.
- At each house, the robber has two choices: rob the current house (and take the money from two houses back) or skip it (and take the money from the previous house).
- By only keeping track of the last two maximum values, we optimize the space complexity from O(n) to O(1).
- The solution efficiently computes the maximum amount by making locally optimal choices at each step, which leads to a globally optimal solution.