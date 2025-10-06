# 200. Number of Islands

## Problem Statement
Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return the number of islands.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

### Examples

**Example 1:**
```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

**Example 2:**
```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

### Constraints:
- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.

## Solution
```java
public class Solution {
    private static final int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    public int numIslands(char[][] grid) {
        int ROWS = grid.length, COLS = grid[0].length;
        int islands = 0;

        // Iterate through each cell in the grid
        for (int r = 0; r < ROWS; r++) {
            for (int c = 0; c < COLS; c++) {
                if (grid[r][c] == '1') {
                    // Start DFS when we find land
                    dfs(grid, r, c);
                    islands++;
                }
            }
        }

        return islands;
    }

    private void dfs(char[][] grid, int r, int c) {
        // Check boundaries and if current cell is water
        if (r < 0 || c < 0 || r >= grid.length ||
            c >= grid[0].length || grid[r][c] == '0') {
            return;
        }

        // Mark the cell as visited by setting it to '0'
        grid[r][c] = '0';
        
        // Explore all four directions
        for (int[] dir : directions) {
            dfs(grid, r + dir[0], c + dir[1]);
        }
    }
}
```

## Approach
1. **Initialization**: Define the four possible directions to move (up, down, left, right).
2. **Grid Traversal**: Iterate through each cell in the grid.
3. **Island Discovery**: When a land cell (`'1'`) is found:
   - Increment the island count.
   - Perform DFS to mark all connected land cells as visited by setting them to `'0'`.
4. **DFS Implementation**:
   - Check if the current cell is out of bounds or water.
   - Mark the current cell as visited.
   - Recursively visit all four neighboring cells.

## Complexity
- **Time Complexity**: O(m × n), where m is the number of rows and n is the number of columns. We visit every cell once.
- **Space Complexity**: O(m × n) in the worst case due to the recursion stack when the grid is filled with land.

## Edge Cases
- Grid with no islands (all water).
- Grid with a single cell that is land.
- Grid with a single cell that is water.
- Large grid (300x300) with alternating land and water.
- Grid with one large island covering all cells.

## Key Insights
- The problem can be modeled as finding connected components in a graph.
- DFS is suitable for exploring all connected land cells.
- By marking visited cells as water (`'0'`), we avoid using extra space for a visited matrix.
- The solution efficiently handles the grid in-place, modifying it during traversal.