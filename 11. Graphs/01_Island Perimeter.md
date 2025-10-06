# 463. Island Perimeter

## Problem Statement
You are given a 2D integer array `grid` where `grid[i][j]` is 1 if it represents land and 0 if it represents water. The grid is completely surrounded by water, and there is exactly one island (a connected set of land cells). The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

### Examples

**Example 1:**
```
Input: grid = [[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]
Output: 16
Explanation: The perimeter is the 16 yellow stripes in the image below.
```

**Example 2:**
```
Input: grid = [[1]]
Output: 4
```

**Example 3:**
```
Input: grid = [[1,0]]
Output: 4
```

### Constraints:
- `grid.length == n`
- `grid[i].length == m`
- `1 <= n, m <= 100`
- `grid[i][j]` is `0` or `1`.
- There is exactly one island in `grid`.

## Solution
```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        int rows = grid.length;
        if (rows == 0) return 0;
        int cols = grid[0].length;
        int perimeter = 0;

        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (grid[r][c] == 1) {
                    // Each land cell contributes 4 sides initially
                    perimeter += 4;
                    // If there's land above, subtract 2 (shared edge counted twice)
                    if (r > 0 && grid[r - 1][c] == 1) perimeter -= 2;
                    // If there's land to the left, subtract 2 (shared edge counted twice)
                    if (c > 0 && grid[r][c - 1] == 1) perimeter -= 2;
                }
            }
        }
        return perimeter;
    }
}
```

## Approach
1. **Initialize Perimeter**: Start with a perimeter of 0.
2. **Iterate Through Grid**: For each cell in the grid:
   - If the cell is land (1), add 4 to the perimeter (4 sides of the cell).
   - Check the cell above and to the left:
     - If there's a land cell above, subtract 2 from the perimeter (shared edge is counted twice).
     - If there's a land cell to the left, subtract 2 from the perimeter (shared edge is counted twice).
3. **Return Result**: The final value of the perimeter after processing all cells.

## Complexity
- **Time Complexity**: O(n * m), where n is the number of rows and m is the number of columns in the grid. We visit each cell exactly once.
- **Space Complexity**: O(1), as we only use a constant amount of extra space.

## Edge Cases
- A single cell island should return a perimeter of 4.
- A single row or column of land cells should have a perimeter of 2 * number of land cells + 2.
- The entire grid filled with land should have a perimeter of 2 * (rows + columns).
- A grid with the maximum size (100x100) should be handled efficiently.
- A grid with the minimum size (1x1) should return the correct perimeter.

## Key Insights
- Each land cell contributes 4 to the perimeter.
- Each shared edge between two land cells reduces the total perimeter by 2 (one from each cell).
- By only checking the top and left neighbors, we avoid double-counting shared edges.
- The approach efficiently calculates the perimeter in a single pass through the grid.