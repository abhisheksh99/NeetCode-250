# 1140. Stone Game II

## Problem Statement
Alice and Bob continue their games with piles of stones. There are a number of piles arranged in a row, and each pile has a positive integer number of stones `piles[i]`.

The objective of the game is to end with the most stones. Alice and Bob take turns, with Alice starting first. Initially, `M = 1`.

On each player's turn, that player can take all the stones in the first `X` remaining piles, where `1 <= X <= 2M`. Then, we set `M = max(M, X)`.

The game continues until all the stones have been taken.

Assuming Alice and Bob play optimally, return the maximum number of stones Alice can get.

## Examples

### Example 1:
```
Input: piles = [2,7,9,4,4]
Output: 10
Explanation:  If Alice takes one pile at the beginning, then Bob takes two piles, then Alice takes 2 piles again. Alice can get 2 + 4 + 4 = 10 piles in total.
```

### Example 2:
```
Input: piles = [1,2,3,4,5,100]
Output: 104
```

## Approach
We use memoization (top-down dynamic programming) to solve this problem:
1. Calculate suffix sums to efficiently compute the sum of remaining stones
2. Use a 2D DP array where `dp[i][M]` represents the maximum stones Alice can get starting from index `i` with the current `M`
3. For each position and M value, try all possible X values (from 1 to 2M)
4. The current player's stones = sum of remaining stones - opponent's optimal stones
5. Memoize results to avoid recomputation

## Solution Code
```java
class Solution {
    public int stoneGameII(int[] piles) {
        int n = piles.length;
        int[][] dp = new int[n][n + 1];
        for (int[] row : dp) Arrays.fill(row, -1);

        // Calculate suffix sums for efficient range sum queries
        int[] suffixSum = new int[n + 1];
        for (int i = n - 1; i >= 0; i--) {
            suffixSum[i] = suffixSum[i + 1] + piles[i];
        }

        // Start the game from the first pile with M = 1
        return dfs(0, 1, piles, dp, suffixSum);
    }

    private int dfs(int i, int M, int[] piles, int[][] dp, int[] suffixSum) {
        int n = piles.length;
        // Base case: no more stones left
        if (i >= n) return 0;
        
        // If we've already computed this state, return the memoized result
        if (dp[i][M] != -1) return dp[i][M];

        int maxStones = 0;
        // Try all possible X from 1 to 2M
        for (int x = 1; x <= 2 * M; x++) {
            // If taking x piles would go out of bounds, stop
            if (i + x > n) break;
            
            // Calculate opponent's optimal stones if we take x piles
            int opponentStones = dfs(i + x, Math.max(M, x), piles, dp, suffixSum);
            
            // Current player's stones = remaining stones - opponent's stones
            int currentStones = suffixSum[i] - opponentStones;
            
            // Update maximum stones for current player
            maxStones = Math.max(maxStones, currentStones);
        }

        // Memoize the result
        dp[i][M] = maxStones;
        return maxStones;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n³) - We have n × n states, and for each state, we do O(n) work
- **Space Complexity**: O(n²) - For the DP table and suffix sum array

## Key Insights
1. **Suffix Sums**: Precompute suffix sums to quickly calculate the sum of remaining stones
2. **State Representation**: `dp[i][M]` represents the maximum stones Alice can get starting from index `i` with current `M`
3. **Game Theory**: Both players play optimally, so we consider the opponent's optimal move in our calculation
4. **Memoization**: Store computed results to avoid redundant calculations

## Edge Cases
1. Single pile (Alice takes it all)
2. Two piles with M=1 (Alice can take 1 or 2 piles)
3. All piles have the same value
4. Large number of piles (tests efficiency)
5. Piles with large values (tests integer overflow handling)

## Follow-up
How would you modify the solution to handle the case where the number of piles can be up to 1000 with each pile having up to 10^4 stones?