# 417. Pacific Atlantic Water Flow

## Problem Statement
Given an `m x n` matrix of non-negative integers representing the height of each unit cell in a continent, the "Pacific ocean" touches the left and top edges of the matrix and the "Atlantic ocean" touches the right and bottom edges.

Water can only flow in four directions (up, down, left, or right) from a cell to another one with height equal or lower.

Find the list of grid coordinates where water can flow to both the Pacific and Atlantic ocean.

### Examples

**Example 1:**
```
Input: heights = [
  [1,2,2,3,5],
  [3,2,3,4,4],
  [2,4,5,3,1],
  [6,7,1,4,5],
  [5,1,1,2,4]
]
Output: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
```

**Example 2:**
```
Input: heights = [[2,1],[1,2]]
Output: [[0,0],[0,1],[1,0],[1,1]]
```

### Constraints:
- `m == heights.length`
- `n == heights[r].length`
- `1 <= m, n <= 200`
- `0 <= heights[r][c] <= 10^5`

## Solution
```java
import java.util.*;

class Solution {
    private int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    
    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        if (heights == null || heights.length == 0) return new ArrayList<>();
        
        int ROWS = heights.length, COLS = heights[0].length;
        boolean[][] pacific = new boolean[ROWS][COLS];
        boolean[][] atlantic = new boolean[ROWS][COLS];
        
        // Start DFS from all cells adjacent to the oceans
        for (int c = 0; c < COLS; c++) {
            dfs(0, c, pacific, heights);         // Top row (Pacific)
            dfs(ROWS - 1, c, atlantic, heights); // Bottom row (Atlantic)
        }
        
        for (int r = 0; r < ROWS; r++) {
            dfs(r, 0, pacific, heights);         // Left column (Pacific)
            dfs(r, COLS - 1, atlantic, heights); // Right column (Atlantic)
        }
        
        // Find cells that can reach both oceans
        List<List<Integer>> result = new ArrayList<>();
        for (int r = 0; r < ROWS; r++) {
            for (int c = 0; c < COLS; c++) {
                if (pacific[r][c] && atlantic[r][c]) {
                    result.add(Arrays.asList(r, c));
                }
            }
        }
        
        return result;
    }
    
    private void dfs(int r, int c, boolean[][] ocean, int[][] heights) {
        // Mark current cell as reachable
        ocean[r][c] = true;
        
        // Check all four directions
        for (int[] dir : directions) {
            int nr = r + dir[0];
            int nc = c + dir[1];
            
            // Check if the next cell is within bounds, not visited, and water can flow to it
            if (nr >= 0 && nr < heights.length &&
                nc >= 0 && nc < heights[0].length &&
                !ocean[nr][nc] && 
                heights[nr][nc] >= heights[r][c]) {
                
                dfs(nr, nc, ocean, heights);
            }
        }
    }
}
```

## Approach
1. **Initialization**: 
   - Create two boolean matrices `pacific` and `atlantic` to track which cells can reach each ocean.
   
2. **DFS from Ocean Borders**:
   - Start DFS from all cells adjacent to the Pacific Ocean (top and left edges).
   - Start DFS from all cells adjacent to the Atlantic Ocean (bottom and right edges).
   - During each DFS, mark cells as reachable if water can flow to them (i.e., if their height is greater than or equal to the current cell).

3. **Find Intersection**:
   - After both DFS traversals, find all cells that are marked as reachable in both `pacific` and `atlantic` matrices.
   - These cells can flow to both oceans.

## Complexity
- **Time Complexity**: O(m × n), where m is the number of rows and n is the number of columns. Each cell is visited at most twice (once for each ocean).
- **Space Complexity**: O(m × n) for the two boolean matrices and the recursion stack in the worst case.

## Edge Cases
- Empty grid.
- Grid with only one cell.
- Grid where all cells have the same height.
- Grid with maximum dimensions (200x200).
- Grid with varying heights where water can flow in complex paths.

## Key Insights
- The problem can be efficiently solved by working backwards from the ocean borders.
- Using DFS to explore all reachable cells from each ocean is more efficient than checking every cell.
- The solution leverages the fact that water flows from higher or equal heights to lower or equal heights.
- The intersection of cells reachable from both oceans gives the final result.
- The solution is optimal as it visits each cell a constant number of times.