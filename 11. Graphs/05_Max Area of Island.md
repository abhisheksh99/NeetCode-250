# 695. Max Area of Island

## Problem Statement
You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected 4-directionally (horizontal or vertical). You may assume all four edges of the grid are surrounded by water.

The **area** of an island is the number of cells with a value `1` in the island.

Return the maximum **area** of an island in `grid`. If there is no island, return `0`.

### Examples

**Example 1:**
```
Input: grid = [
  [0,0,1,0,0,0,0,1,0,0,0,0,0],
  [0,0,0,0,0,0,0,1,1,1,0,0,0],
  [0,1,1,0,1,0,0,0,0,0,0,0,0],
  [0,1,0,0,1,1,0,0,1,0,1,0,0],
  [0,1,0,0,1,1,0,0,1,1,1,0,0],
  [0,0,0,0,0,0,0,0,0,0,1,0,0],
  [0,0,0,0,0,0,0,1,1,1,0,0,0],
  [0,0,0,0,0,0,0,1,1,0,0,0,0]
]
Output: 6
Explanation: The answer is not 11, because the island must be connected 4-directionally.
```

**Example 2:**
```
Input: grid = [[0,0,0,0,0,0,0,0]]
Output: 0
```

### Constraints:
- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `grid[i][j]` is either `0` or `1`.

## Solution
```java
public class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        if (grid == null || grid.length == 0) return 0;
        int maxArea = 0;
        
        // Iterate through each cell in the grid
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[i].length; j++) {
                if (grid[i][j] == 1) {
                    // Calculate area of current island and update maxArea
                    int area = dfs(grid, i, j);
                    maxArea = Math.max(maxArea, area);
                }
            }
        }
        return maxArea;
    }

    private int dfs(int[][] grid, int i, int j) {
        // Check if current cell is out of bounds or is water
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[i].length || grid[i][j] == 0) {
            return 0;
        }
        
        // Mark the cell as visited by setting it to 0
        grid[i][j] = 0;
        
        // Current cell contributes 1 to the area
        int area = 1;
        
        // Explore all four directions and accumulate the area
        area += dfs(grid, i + 1, j); // down
        area += dfs(grid, i - 1, j); // up
        area += dfs(grid, i, j + 1); // right
        area += dfs(grid, i, j - 1); // left
        
        return area;
    }
}
```

## Approach
1. **Initialization**: Check for edge cases (null or empty grid).
2. **Grid Traversal**: Iterate through each cell in the grid.
3. **Island Discovery**: When a land cell (`1`) is found:
   - Calculate the area of the island using DFS.
   - Update the maximum area found so far.
4. **DFS Implementation**:
   - Check if the current cell is out of bounds or water.
   - Mark the current cell as visited by setting it to `0`.
   - Initialize area as 1 (for the current cell).
   - Recursively calculate the area in all four directions.
   - Return the total area of the current island.

## Complexity
- **Time Complexity**: O(m × n), where m is the number of rows and n is the number of columns. We visit each cell once.
- **Space Complexity**: O(m × n) in the worst case due to the recursion stack when the grid is filled with land.

## Edge Cases
- Grid with no islands (all water).
- Grid with a single cell that is land.
- Grid with a single cell that is water.
- Grid with one large island covering all cells.
- Grid with multiple islands of varying sizes.
- Maximum size grid (50x50) with alternating land and water.

## Key Insights
- The problem is an extension of the "Number of Islands" problem.
- Uses Depth-First Search (DFS) to explore connected land cells.
- Modifies the grid in-place to mark visited cells, avoiding extra space for a visited matrix.
- Accumulates the area by adding 1 for each land cell and the areas from all four directions.
- Efficiently tracks the maximum area found during the grid traversal.