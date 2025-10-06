# 746. Min Cost Climbing Stairs

## Problem Statement
You are given an integer array `cost` where `cost[i]` is the cost of `i-th` step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index `0`, or the step with index `1`.

Return the minimum cost to reach the top of the floor (beyond the last index of the array).

### Examples

**Example 1:**
```
Input: cost = [10,15,20]
Output: 15
Explanation: You will start at index 1.
- Pay 15 and climb two steps to reach the top.
The total cost is 15.
```

**Example 2:**
```
Input: cost = [1,100,1,1,1,100,1,1,100,1]
Output: 6
Explanation: You will start at index 0.
- Pay 1 and climb two steps to reach index 2.
- Pay 1 and climb two steps to reach index 4.
- Pay 1 and climb two steps to reach index 6.
- Pay 1 and climb one step to reach index 7.
- Pay 1 and climb two steps to reach index 9.
- Pay 1 and climb one step to reach the top.
The total cost is 6.
```

### Constraints:
- `2 <= cost.length <= 1000`
- `0 <= cost[i] <= 999`

## Solution
```java
class Solution {
   public static int minCostClimbingStairs(int[] cost) {
        // Base cases
        if (cost.length == 1) return cost[0];
        if (cost.length == 2) return Math.min(cost[0], cost[1]);

        // Use two variables to store the minimum cost to reach previous two steps
        int first = cost[0];  // Minimum cost to reach step 0
        int second = cost[1]; // Minimum cost to reach step 1
        
        // Iterate from step 2 to the end
        for (int i = 2; i < cost.length; i++) {
            // Current cost is the cost of current step plus the minimum cost to reach it
            int current = Math.min(first, second) + cost[i];
            
            // Update the previous two steps' costs
            first = second;
            second = current;
        }
        
        // The minimum cost to reach the top is the minimum of the last two steps
        // because we can take either 1 or 2 steps from there
        return Math.min(first, second);
    }
}
```

## Approach
1. **Dynamic Programming (Space Optimized)**:
   - We use two variables `first` and `second` to keep track of the minimum cost to reach the previous two steps.
   - For each step `i`, the minimum cost to reach it is the cost of the current step plus the minimum cost to reach either of the two previous steps.
   - We update `first` and `second` in each iteration to always have the minimum costs for the last two steps.
   - The final result is the minimum of the last two steps' costs since we can reach the top from either of them.

2. **Base Cases**:
   - If there's only one step, the cost is `cost[0]`.
   - If there are two steps, the minimum cost is the minimum of `cost[0]` and `cost[1]` since we can start from either.

## Complexity
- **Time Complexity**: O(n)
  - We iterate through the cost array once, performing constant time operations in each iteration.
- **Space Complexity**: O(1)
  - We use only a constant amount of extra space (two variables) regardless of the input size.

## Edge Cases
- `cost = [10, 15, 20]` → `15` (start from index 1, pay 15, take two steps to the top)
- `cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]` → `6` (optimal path: 0 → 2 → 4 → 6 → 7 → 9 → top)
- `cost = [0, 0, 0, 0]` → `0` (no cost to reach the top)
- `cost = [1, 2]` → `1` (start from index 0, pay 1, take one step to the top)

## Key Insights
- The problem can be solved using dynamic programming by breaking it down into smaller subproblems.
- The minimum cost to reach step `i` depends only on the minimum costs to reach steps `i-1` and `i-2`.
- By using only two variables instead of an array, we optimize the space complexity from O(n) to O(1).
- The final answer is the minimum of the last two steps' costs since we can reach the top from either of them in one or two steps.