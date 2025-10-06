# 286. Walls and Gates

## Problem Statement
You are given an `m x n` grid `rooms` initialized with these three possible values:
- `-1` - A wall or an obstacle.
- `0` - A gate.
- `INF` - Infinity means an empty room. We use the value `2^31 - 1 = 2147483647` to represent `INF` as you may assume that the distance to a gate is less than `2147483647`.

Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with `INF`.

### Examples

**Example 1:**
```
Input: 
[
  [2147483647, -1, 0, 2147483647],
  [2147483647, 2147483647, 2147483647, -1],
  [2147483647, -1, 2147483647, -1],
  [0, -1, 2147483647, 2147483647]
]

Output:
[
  [3, -1, 0, 1],
  [2, 2, 1, -1],
  [1, -1, 2, -1],
  [0, -1, 3, 4]
]
```

**Example 2:**
```
Input: grid = [[-1]]
Output: [[-1]]
```

### Constraints:
- `m == rooms.length`
- `n == rooms[i].length`
- `1 <= m, n <= 250`
- `rooms[i][j]` is `-1`, `0`, or `2^31 - 1`.

## Solution
```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    public void wallsAndGates(int[][] rooms) {
        if (rooms == null || rooms.length == 0) return;

        int rows = rooms.length, cols = rooms[0].length;
        Queue<int[]> queue = new LinkedList<>();

        // Enqueue all gate cells (value 0)
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (rooms[r][c] == 0) {
                    queue.offer(new int[]{r, c});
                }
            }
        }

        // Directions: down, up, right, left
        int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        int distance = 0;

        // BFS from all gates simultaneously
        while (!queue.isEmpty()) {
            distance++;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] point = queue.poll();
                int row = point[0], col = point[1];

                // Explore all four directions
                for (int[] dir : directions) {
                    int r = row + dir[0], c = col + dir[1];
                    
                    // Skip if out of bounds, is a wall, or already has a smaller distance
                    if (r < 0 || c < 0 || r >= rows || c >= cols || 
                        rooms[r][c] == -1 || rooms[r][c] != Integer.MAX_VALUE) {
                        continue;
                    }
                    
                    // Update distance and enqueue the cell
                    rooms[r][c] = distance;
                    queue.offer(new int[]{r, c});
                }
            }
        }
    }
}
```

## Approach
1. **Initialization**: Check for edge cases (null or empty grid).
2. **Gate Collection**: Enqueue all gate positions (value 0) into a queue.
3. **BFS Traversal**:
   - For each level (distance), process all cells in the current level.
   - For each cell, explore its four neighboring cells.
   - If a neighboring cell is an empty room (INF), update its distance and enqueue it.
4. **Termination**: The BFS completes when all reachable rooms have been processed.

## Complexity
- **Time Complexity**: O(m × n), where m is the number of rows and n is the number of columns. Each cell is processed once.
- **Space Complexity**: O(m × n) in the worst case, which is used by the queue when all gates are at the same level.

## Edge Cases
- Empty grid.
- Grid with no gates.
- Grid with no walls.
- Grid with all gates.
- Maximum size grid (250x250).
- Rooms that are unreachable from any gate.

## Key Insights
- The problem is a classic multi-source BFS problem.
- Starting BFS from all gates simultaneously ensures the shortest path to the nearest gate.
- The grid is modified in-place to store the distances.
- The BFS naturally finds the shortest path in an unweighted grid.
- The solution efficiently handles large grids by processing each cell exactly once.