# 64. Minimum Path Sum

## Problem Statement
Given a `m x n` grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path. You can only move either down or right at any point in time.

## Examples

### Example 1:
```
Input: grid = [
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.
```

### Example 2:
```
Input: grid = [[1,2,3],[4,5,6]]
Output: 12
```

## Approach
We use dynamic programming with a bottom-up approach:
1. Create a DP table with one extra row and column filled with `Integer.MAX_VALUE`
2. Set the starting point (bottom-right of the extra space) to 0
3. Process the grid from bottom-right to top-left
4. For each cell, the minimum path sum is the current cell's value plus the minimum of the cell below or to the right

## Solution Code
```java
public class Solution {
    public int minPathSum(int[][] grid) {
        int ROWS = grid.length, COLS = grid[0].length;
        int[][] dp = new int[ROWS + 1][COLS + 1];

        // Initialize DP table with maximum values
        for (int r = 0; r <= ROWS; r++) {
            for (int c = 0; c <= COLS; c++) {
                dp[r][c] = Integer.MAX_VALUE;
            }
        }
        // Base case: starting point (bottom-right of extra space)
        dp[ROWS - 1][COLS] = 0;

        // Fill DP table from bottom-right to top-left
        for (int r = ROWS - 1; r >= 0; r--) {
            for (int c = COLS - 1; c >= 0; c--) {
                // Current cell's value + minimum of right or bottom cell
                dp[r][c] = grid[r][c] + Math.min(dp[r + 1][c], dp[r][c + 1]);
            }
        }

        return dp[0][0];  // Result is at the starting position
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(m×n) - We process each cell exactly once
- **Space Complexity**: O(m×n) - For the DP table (can be optimized to O(n) using a 1D array)

## Space Optimization
The space complexity can be optimized to O(n) by using a 1D array:
```java
public int minPathSum(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    int[] dp = new int[n];
    
    // Initialize last row
    dp[n-1] = grid[m-1][n-1];
    for (int j = n-2; j >= 0; j--) {
        dp[j] = grid[m-1][j] + dp[j+1];
    }
    
    // Fill remaining rows
    for (int i = m-2; i >= 0; i--) {
        dp[n-1] += grid[i][n-1];  // Last column
        for (int j = n-2; j >= 0; j--) {
            dp[j] = grid[i][j] + Math.min(dp[j], dp[j+1]);
        }
    }
    
    return dp[0];
}
```

## Key Insights
1. **Bottom-up Approach**: We start from the destination and work backwards to the start
2. **DP Initialization**: The extra row and column with `Integer.MAX_VALUE` act as boundaries
3. **State Transition**: Each cell's value is the current cell's value plus the minimum of the cell below or to the right
4. **Boundary Handling**: The extra column and row prevent index out of bounds exceptions

## Edge Cases
1. Single row or column grid
2. Minimum grid size (1x1)
3. Grid with all same values
4. Large grid sizes

## Follow-up
How would you modify the solution if you could move in all four directions (up, down, left, right) but cannot visit the same cell twice?