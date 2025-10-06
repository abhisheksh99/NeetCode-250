# 877. Stone Game

## Problem Statement
Alice and Bob play a game with piles of stones. There are an even number of piles arranged in a row, and each pile has a positive integer number of stones `piles[i]`.

The objective of the game is to end with the most stones. The total number of stones is odd, so there are no ties.

Alice and Bob take turns, with Alice starting first. Each turn, a player takes the entire pile of stones from either the beginning or the end of the row. This continues until there are no more piles left, at which point the player with the most stones wins.

Assuming Alice and Bob play optimally, return `true` if Alice wins the game, or `false` if Bob wins.

## Examples

### Example 1:
```
Input: piles = [5,3,4,5]
Output: true
Explanation: 
- Alice starts first, and can only take the first 5 or the last 5.
- If she takes the first 5, she's left with [3,4,5].
- If Bob takes 3, then she takes 5 and wins with 10 points.
- If Bob takes 5, then she takes 4 and wins with 9 points.
- So, Alice can always win.
```

### Example 2:
```
Input: piles = [3,7,2,3]
Output: true
```

## Approach
We use dynamic programming to solve this problem:
1. Create a 2D DP array where `dp[i][j]` represents the maximum difference in stones the current player can achieve over the other player for the subarray `piles[i...j]`
2. Base case: When there's only one pile, the player takes it (`dp[i][i] = piles[i]`)
3. For each subarray length from 2 to n, calculate the optimal choice for each possible subarray
4. The current player can choose either the leftmost or rightmost pile, and the opponent will play optimally in the remaining subarray
5. The final result is whether the difference at `dp[0][n-1]` is positive (Alice wins)

## Solution Code
```java
class Solution {
    public boolean stoneGame(int[] piles) {
        int n = piles.length;
        int[][] dp = new int[n][n];

        // Base cases: when the interval length is 1, dp[i][i] = piles[i]
        for (int i = 0; i < n; i++) {
            dp[i][i] = piles[i];
        }

        // Bottom-up calculation for intervals of length 2 to n
        for (int length = 2; length <= n; length++) {
            for (int i = 0; i <= n - length; i++) {
                int j = i + length - 1;
                // If player picks left pile
                int pickLeft = piles[i] - dp[i + 1][j];
                // If player picks right pile
                int pickRight = piles[j] - dp[i][j - 1];
                // Choose the optimal move
                dp[i][j] = Math.max(pickLeft, pickRight);
            }
        }

        // If the first player's score difference is positive, Alice wins
        return dp[0][n - 1] > 0;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n²) - We fill an n×n DP table
- **Space Complexity**: O(n²) - For the DP table (can be optimized to O(n) with 1D array)

## Space Optimization
We can optimize the space to O(n) by using a 1D array:
```java
public boolean stoneGame(int[] piles) {
    int n = piles.length;
    int[] dp = new int[n];
    
    // Initialize dp array with the values of individual piles
    System.arraycopy(piles, 0, dp, 0, n);
    
    for (int length = 2; length <= n; length++) {
        for (int i = 0; i <= n - length; i++) {
            int j = i + length - 1;
            dp[i] = Math.max(piles[i] - dp[i + 1], 
                           piles[j] - (length > 2 ? dp[i] : piles[i]));
        }
    }
    
    return dp[0] > 0;
}
```

## Key Insights
1. **Optimal Substructure**: The solution to the problem can be constructed from solutions to its subproblems
2. **State Representation**: `dp[i][j]` represents the maximum difference the current player can achieve for the subarray `piles[i...j]`
3. **Game Theory**: Both players play optimally, so we consider the opponent's optimal move in our calculation
4. **Base Case**: When there's only one pile, the player takes it and the difference is the value of that pile

## Edge Cases
1. Minimum number of piles (2 piles)
2. Maximum number of piles (500 piles as per constraints)
3. All piles have the same value
4. Alternating high and low values
5. Large input size (tests efficiency)

## Follow-up
How would you modify the solution to return the actual moves Alice should make to guarantee a win (if possible)?