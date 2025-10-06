# 63. Unique Paths II

## Problem Statement
A robot is located at the top-left corner of a `m x n` grid. The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid. Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and space are marked as `1` and `0` respectively in the grid.

## Examples

### Example 1:
```
Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
Output: 2
Explanation:
There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```

### Example 2:
```
Input: obstacleGrid = [[0,1],[0,0]]
Output: 1
```

## Approach
We use dynamic programming with space optimization to solve this problem. The key insights are:
1. If a cell has an obstacle, there are 0 ways to reach it.
2. The number of ways to reach any cell is the sum of ways to reach the cell above it and the cell to its left.
3. We can optimize space by using a 1D array instead of a 2D grid.

## Solution Code
```java
public class Solution {
    public int uniquePathsWithObstacles(int[][] grid) {
        int M = grid.length, N = grid[0].length;
        int[] dp = new int[N + 1];  // Extra space for easier calculation
        dp[N - 1] = 1;  // Starting point (bottom-right corner)

        // Process from bottom-right to top-left
        for (int r = M - 1; r >= 0; r--) {
            for (int c = N - 1; c >= 0; c--) {
                if (grid[r][c] == 1) {
                    // If current cell is an obstacle, no path can go through it
                    dp[c] = 0;
                } else {
                    // Number of paths = paths from right + paths from below
                    dp[c] += dp[c + 1];
                }
            }
        }

        return dp[0];  // Result is stored in the first cell
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(m×n) - We need to process each cell exactly once.
- **Space Complexity**: O(n) - We use a 1D array of size n (number of columns) for DP.

## Edge Cases
1. Grid with obstacle at start or end (should return 0)
2. Single row or column with obstacles
3. Large grid (tests efficiency)
4. Grid with no obstacles (should match unique paths I solution)

## Key Insights
1. **Bottom-up approach**: We start from the destination and work backwards to the start.
2. **Space optimization**: We only need to maintain a 1D array instead of a full 2D grid.
3. **In-place modification**: The DP array is updated in place as we process each row.
4. **Obstacle handling**: Any cell with an obstacle immediately sets its path count to 0.

## Alternative Approaches
1. **2D DP Table**: Easier to understand but uses O(m×n) space.
2. **Recursion with Memoization**: More intuitive but has higher space complexity due to recursion stack.
3. **Math-based solution**: For the case without obstacles, but not applicable here.

## Follow-up
How would you modify the solution if the robot can move in all four directions (up, down, left, right) but cannot visit the same cell twice?