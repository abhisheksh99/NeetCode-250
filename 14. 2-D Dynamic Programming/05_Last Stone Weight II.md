# 1049. Last Stone Weight II

## Problem Statement
You are given an array of integers `stones` where `stones[i]` is the weight of the `i-th` stone. We are playing a game with the stones. On each turn, we choose any two stones and smash them together. Suppose the stones have weights `x` and `y` with `x <= y`. The result of this smash is:
- If `x == y`, both stones are destroyed
- If `x != y`, the stone of weight `x` is destroyed, and the stone of weight `y` has new weight `y - x`

Return the smallest possible weight of the left stone after all possible sequences of smashing. If there are no stones left, return `0`.

## Examples

### Example 1:
```
Input: stones = [2,7,4,1,8,1]
Output: 1
Explanation:
We can combine 2 and 4 to get 2, so the array converts to [2,7,1,8,1].
Then we can combine 7 and 8 to get 1, so the array converts to [2,1,1,1].
Then we can combine 2 and 1 to get 1, so the array converts to [1,1,1].
Then we can combine 1 and 1 to get 0, so the array converts to [1].
The last remaining stone is 1.
```

### Example 2:
```
Input: stones = [31,26,33,21,40]
Output: 5
```

## Approach
This problem can be reduced to a variation of the 0/1 Knapsack problem:
1. Calculate the total sum of all stones
2. Find the maximum weight we can achieve that is less than or equal to half of the total sum
3. The result will be `total_sum - 2 * max_weight`

The intuition is that we want to partition the stones into two groups with weights as close as possible to each other. The minimum difference between these two groups will be our answer.

## Solution Code
```java
public class Solution {
    public int lastStoneWeightII(int[] stones) {
        // Calculate the total sum of all stones
        int stoneSum = 0;
        for (int stone : stones) {
            stoneSum += stone;
        }
        
        // Target is half of the total sum (rounded down)
        int target = stoneSum / 2;
        int n = stones.length;

        // dp[i][j] represents the maximum weight achievable with first i stones and capacity j
        int[][] dp = new int[n + 1][target + 1];

        // Fill the DP table
        for (int i = 1; i <= n; i++) {
            for (int t = 0; t <= target; t++) {
                if (t >= stones[i - 1]) {
                    // Choose to take or not take the current stone
                    dp[i][t] = Math.max(dp[i - 1][t], 
                                      dp[i - 1][t - stones[i - 1]] + stones[i - 1]);
                } else {
                    // Cannot take the current stone as it exceeds capacity
                    dp[i][t] = dp[i - 1][t];
                }
            }
        }

        // The result is the difference between the two partitions
        return stoneSum - 2 * dp[n][target];
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n × sum/2) - Where n is the number of stones and sum is the total weight of all stones
- **Space Complexity**: O(n × sum/2) - For the DP table

## Space Optimization
We can optimize the space to O(sum/2) by using a 1D array:
```java
public int lastStoneWeightII(int[] stones) {
    int sum = 0;
    for (int stone : stones) {
        sum += stone;
    }
    int target = sum / 2;
    int[] dp = new int[target + 1];
    
    for (int stone : stones) {
        for (int t = target; t >= stone; t--) {
            dp[t] = Math.max(dp[t], dp[t - stone] + stone);
        }
    }
    
    return sum - 2 * dp[target];
}
```

## Key Insights
1. **Problem Reduction**: The problem reduces to finding a partition of stones into two subsets with minimum difference
2. **DP State**: `dp[i][j]` represents the maximum weight achievable with first `i` stones and capacity `j`
3. **Transition**: For each stone, we decide whether to include it or not in our subset
4. **Final Calculation**: The result is `total_sum - 2 * maximum_achievable_weight`

## Edge Cases
1. Single stone (return the stone's weight)
2. All stones have the same weight
3. Sum of stones is odd/even
4. Large input size (tests efficiency)
5. All stones can be paired to 0

## Follow-up
How would you modify the solution to return the actual partition of stones that results in the minimum possible last stone weight?