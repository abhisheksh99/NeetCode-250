# 994. Rotting Oranges

## Problem Statement
You are given an `m x n` grid where each cell can have one of three values:
- `0` representing an empty cell,
- `1` representing a fresh orange, or
- `2` representing a rotten orange.

Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return `-1`.

### Examples

**Example 1:**
```
Input: grid = [
  [2,1,1],
  [1,1,0],
  [0,1,1]
]
Output: 4
```

**Example 2:**
```
Input: grid = [
  [2,1,1],
  [0,1,1],
  [1,0,1]
]
Output: -1
Explanation: The orange in the bottom left corner (row 2, column 0) is never rotten.
```

**Example 3:**
```
Input: grid = [[0,2]]
Output: 0
Explanation: Since there are no fresh oranges at minute 0, the answer is just 0.
```

### Constraints:
- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10`
- `grid[i][j]` is `0`, `1`, or `2`.
- At least one cell in the grid has a value of `2`.

## Solution
```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    public int orangesRotting(int[][] grid) {
        if (grid == null || grid.length == 0) return -1;

        int m = grid.length, n = grid[0].length;
        int freshCount = 0;
        Queue<int[]> rottenQueue = new LinkedList<>();

        // Count fresh oranges and enqueue rotten oranges
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    freshCount++;
                } else if (grid[i][j] == 2) {
                    rottenQueue.offer(new int[]{i, j});
                }
            }
        }

        // If no fresh oranges, return 0 immediately
        if (freshCount == 0) return 0;

        int minutes = 0;
        // Directions: down, up, right, left
        int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

        // BFS to rot adjacent oranges
        while (!rottenQueue.isEmpty()) {
            int size = rottenQueue.size();
            boolean rottenInThisMinute = false;
            
            // Process all currently rotten oranges
            for (int i = 0; i < size; i++) {
                int[] rotten = rottenQueue.poll();
                
                // Check all four directions
                for (int[] dir : directions) {
                    int x = rotten[0] + dir[0];
                    int y = rotten[1] + dir[1];
                    
                    // If adjacent cell is a fresh orange, rot it
                    if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1) {
                        grid[x][y] = 2; // Mark as rotten
                        freshCount--;   // Decrease fresh count
                        rottenQueue.offer(new int[]{x, y});
                        rottenInThisMinute = true;
                    }
                }
            }
            
            // Only increment minutes if we rotted any oranges in this minute
            if (rottenInThisMinute) {
                minutes++;
            }
        }

        // If there are still fresh oranges left, return -1
        return freshCount == 0 ? minutes : -1;
    }
}
```

## Approach
1. **Initialization**: 
   - Count the number of fresh oranges and enqueue all initially rotten oranges.
   - If there are no fresh oranges, return 0 immediately.

2. **BFS Traversal**:
   - Use a queue to perform level-order BFS.
   - For each minute, process all currently rotten oranges.
   - For each rotten orange, check its four adjacent cells.
   - If an adjacent cell contains a fresh orange, rot it and enqueue it.
   - Track if any oranges were rotted in the current minute.

3. **Termination**:
   - The BFS completes when there are no more oranges to rot.
   - If all fresh oranges were rotted, return the number of minutes.
   - If any fresh oranges remain, return -1.

## Complexity
- **Time Complexity**: O(m × n), where m is the number of rows and n is the number of columns. Each cell is processed at most once.
- **Space Complexity**: O(m × n) in the worst case, when the queue is filled with all rotten oranges.

## Edge Cases
- Grid with no fresh oranges.
- Grid with all rotten oranges.
- Grid with isolated fresh oranges that can't be rotted.
- Grid with only one cell.
- Maximum size grid (10x10).

## Key Insights
- The problem is a classic BFS problem where we need to find the minimum time to affect all reachable nodes.
- Using BFS ensures that we find the minimum time as it processes all oranges level by level.
- The solution efficiently tracks fresh oranges and stops when no more can be rotted.
- The grid is modified in-place to mark rotted oranges, avoiding the need for additional space.
- The solution handles edge cases like no fresh oranges or all oranges already rotten.