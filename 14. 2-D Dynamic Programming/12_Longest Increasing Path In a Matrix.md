# 329. Longest Increasing Path in a Matrix

## Problem Statement
Given an `m x n` integers `matrix`, return the length of the longest increasing path in `matrix`.

From each cell, you can either move in four directions: left, right, up, or down. You may not move diagonally or move outside the boundary (i.e., wrap-around is not allowed).

## Examples

### Example 1:
```
Input: matrix = [[9,9,4],[6,6,8],[2,1,1]]
Output: 4
Explanation: The longest increasing path is [1, 2, 6, 9].
```

### Example 2:
```
Input: matrix = [[3,4,5],[3,2,6],[2,2,1]]
Output: 4
Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.
```

## Approach
We use memoization (top-down dynamic programming) with depth-first search (DFS) to solve this problem:
1. For each cell in the matrix, perform DFS to find the longest increasing path starting from that cell
2. Use a cache to store already computed results to avoid redundant calculations
3. For each cell, explore all four possible directions (up, down, left, right)
4. If the neighboring cell has a greater value, recursively compute its longest path
5. The result for the current cell is 1 + maximum of all valid neighboring paths

## Solution Code
```java
public class Solution {
    private static final int[][] dirs = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    private int m, n;

    public int longestIncreasingPath(int[][] matrix) {
        if (matrix.length == 0) return 0;
        m = matrix.length; 
        n = matrix[0].length;
        int[][] cache = new int[m][n];
        int ans = 0;
        
        // Find the longest increasing path starting from each cell
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j)
                ans = Math.max(ans, dfs(matrix, i, j, cache));
        return ans;
    }

    private int dfs(int[][] matrix, int i, int j, int[][] cache) {
        // If we've already computed the result for this cell, return it
        if (cache[i][j] != 0) return cache[i][j];
        
        // Explore all four directions
        for (int[] d : dirs) {
            int x = i + d[0], y = j + d[1];
            // Check if the next cell is within bounds and has a greater value
            if (0 <= x && x < m && 0 <= y && y < n && matrix[x][y] > matrix[i][j])
                cache[i][j] = Math.max(cache[i][j], dfs(matrix, x, y, cache));
        }
        
        // Add 1 for the current cell
        return ++cache[i][j];
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(m × n) - Each cell is visited exactly once, and its value is cached
- **Space Complexity**: O(m × n) - For the cache matrix and the recursion stack in the worst case

## Key Insights
1. **Memoization**: The cache stores the length of the longest increasing path starting from each cell
2. **DFS with Pruning**: The algorithm prunes unnecessary paths by checking if the next cell has a greater value
3. **Direction Handling**: The four possible directions are handled using a direction array for cleaner code
4. **Base Case**: The base case is implicitly handled by the cache check and boundary conditions

## Edge Cases
1. Empty matrix (should return 0)
2. Single element matrix (should return 1)
3. All elements the same (longest path is 1)
4. Strictly increasing sequence (longest path is m×n)
5. Large matrix (tests efficiency)

## Follow-up
How would you modify the solution to return the actual longest increasing path instead of just its length?