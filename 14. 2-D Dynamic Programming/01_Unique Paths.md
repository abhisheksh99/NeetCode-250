# 62. Unique Paths

## Problem Statement
A robot is located at the top-left corner of a `m x n` grid. The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid. How many possible unique paths are there?

## Examples

### Example 1:
```
Input: m = 3, n = 7
Output: 28
```

### Example 2:
```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down
```

## Approach
We use dynamic programming to solve this problem:
1. Initialize a 2D DP array where `dp[i][j]` represents the number of ways to reach cell (i, j)
2. The first row and first column can only be reached in one way (all 1's)
3. For other cells, the number of ways is the sum of ways from the top cell and left cell

## Solution Code
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        
        // Initialize all cells to 1 since there's at least one way to reach each cell
        for (int[] row : dp) {
            Arrays.fill(row, 1);
        }
        
        // Fill the DP table
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                // Number of ways to reach current cell = ways from top + ways from left
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        
        return dp[m-1][n-1];  // Bottom-right corner contains the answer
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(m×n) - We need to fill each cell of the m×n grid exactly once
- **Space Complexity**: O(m×n) - For the DP table (can be optimized to O(n) using a single array)

## Space Optimization
The space complexity can be optimized to O(n) by using a single array:
```java
public int uniquePaths(int m, int n) {
    int[] dp = new int[n];
    Arrays.fill(dp, 1);
    
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            dp[j] += dp[j-1];
        }
    }
    
    return dp[n-1];
}
```

## Key Insights
1. **Combinatorics Approach**: The problem can be solved using combinations (m+n-2 choose m-1)
2. **DP Initialization**: First row and column are always 1 since there's only one way to reach them
3. **State Transition**: Each cell's value depends on its top and left neighbors
4. **Boundary Conditions**: Handle cases where m or n is 1 (only one possible path)

## Edge Cases
1. When m = 1 or n = 1 (only one possible path)
2. Minimum grid size (1x1)
3. Large grid sizes (tests efficiency)

## Follow-up
How would you modify the solution if some cells have obstacles (like in Unique Paths II)?