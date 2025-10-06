# 1631. Path With Minimum Effort

## Problem Statement
You are a hiker preparing for an upcoming hike. You are given `heights`, a 2D array of size `rows x columns`, where `heights[row][col]` represents the height of the cell at `(row, col)`. You are situated in the top-left cell, `(0, 0)`, and you hope to travel to the bottom-right cell, `(rows-1, columns-1)` (0-indexed).

You can move up, down, left, or right, and you wish to find a route that requires the minimum effort.

The effort of a route is the maximum absolute difference in heights between two consecutive cells of the route.

Return the minimum effort required to travel from the top-left cell to the bottom-right cell.

### Examples

**Example 1:**
```
Input: heights = [[1,2,2],[3,8,2],[5,3,5]]
Output: 2
Explanation: The route of [1,3,5,3,5] has a maximum absolute difference of 2 in consecutive cells.
This is better than the route of [1,2,2,2,5], where the maximum absolute difference is 3.
```

**Example 2:**
```
Input: heights = [[1,2,3],[3,8,4],[5,3,5]]
Output: 1
Explanation: The route of [1,2,3,4,5] has a maximum absolute difference of 1 in consecutive cells, which is better than route [1,3,5,3,5].
```

### Constraints:
- `rows == heights.length`
- `columns == heights[i].length`
- `1 <= rows, columns <= 100`
- `1 <= heights[i][j] <= 10^6`

## Solution
```java
import java.util.*;

class Solution {
    public int minimumEffortPath(int[][] heights) {
        int m = heights.length, n = heights[0].length;
        int[][] effort = new int[m][n];
        for (int[] row : effort) Arrays.fill(row, Integer.MAX_VALUE);
        effort[0][0] = 0;

        // Min-heap based on effort
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        pq.offer(new int[]{0, 0, 0}); // {effort, x, y}

        int[][] dirs = {{-1,0}, {1,0}, {0,-1}, {0,1}};

        while (!pq.isEmpty()) {
            int[] curr = pq.poll();
            int currEff = curr[0], x = curr[1], y = curr[2];

            // If we've reached the destination, return the effort
            if (x == m - 1 && y == n - 1) return currEff;

            // If we already found a better path, skip
            if (currEff > effort[x][y]) continue;

            // Explore all 4 directions
            for (int[] dir : dirs) {
                int nx = x + dir[0], ny = y + dir[1];
                if (nx >= 0 && nx < m && ny >= 0 && ny < n) {
                    // Calculate the effort for the next cell
                    int nextEff = Math.max(currEff, Math.abs(heights[x][y] - heights[nx][ny]));
                    
                    // If we found a better path to (nx, ny), update and add to queue
                    if (nextEff < effort[nx][ny]) {
                        effort[nx][ny] = nextEff;
                        pq.offer(new int[]{nextEff, nx, ny});
                    }
                }
            }
        }
        return 0; // This line is reached only if the destination is unreachable
    }
}
```

## Approach
1. **Dijkstra's Algorithm**: We use a modified version of Dijkstra's algorithm where:
   - The priority queue is ordered by the current effort (maximum absolute difference so far)
   - We keep track of the minimum effort required to reach each cell
   - We explore the path with the current minimum effort first

2. **Effort Calculation**: 
   - For each move to a neighboring cell, the effort is the maximum of:
     - The current path's maximum effort so far
     - The absolute difference between the current cell and the next cell's heights

3. **Early Termination**: 
   - The algorithm terminates as soon as we reach the destination cell since we're using a min-heap and the first time we reach the destination will be with the minimum possible effort

## Complexity
- **Time Complexity**: O(m*n*log(m*n))
  - Each cell is processed at most once, and each processing involves a heap operation which is O(log(m*n))
- **Space Complexity**: O(m*n)
  - We maintain a 2D array to store the minimum effort for each cell
  - The priority queue can grow up to m*n elements in the worst case

## Edge Cases
- Single cell grid (1x1) - should return 0
- Grid with all cells having the same height - should return 0
- Grid where the only path has increasing heights - effort should be the maximum difference between consecutive cells
- Large grid (100x100) with maximum height differences - should still complete within time limits

## Key Insights
- The problem can be modeled as finding the path with the minimum "bottleneck" edge (maximum height difference)
- Dijkstra's algorithm is suitable because it always expands the most promising path first
- The effort to reach a cell can only stay the same or increase, never decrease, which is why we can use a greedy approach
- The algorithm is similar to finding the shortest path in a graph where edge weights are the height differences between adjacent cells