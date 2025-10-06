# 1584. Min Cost to Connect All Points

## Problem Statement
You are given an array `points` representing integer coordinates of some points on a 2D-plane, where `points[i] = [xi, yi]`.

The cost of connecting two points `[xi, yi]` and `[xj, yj]` is the Manhattan distance between them: `|xi - xj| + |yi - yj|`, where `|val|` denotes the absolute value of `val`.

Return the minimum cost to make all points connected. All points are connected if there is exactly one simple path between any two points.

### Examples

**Example 1:**
```
Input: points = [[0,0],[2,2],[3,10],[5,2],[7,0]]
Output: 20
Explanation: 
We can connect the points as shown above to get the minimum cost of 20.
Notice that there is a unique path between every pair of points.
```

**Example 2:**
```
Input: points = [[3,12],[-2,5],[-4,1]]
Output: 18
```

### Constraints:
- `1 <= points.length <= 1000`
- `-10^6 <= xi, yi <= 10^6`
- All pairs `(xi, yi)` are distinct.

## Solution
```java
import java.util.*;

class Solution {
    public int minCostConnectPoints(int[][] points) {
        int n = points.length;
        // Min-heap based on distance
        PriorityQueue<Point> pq = new PriorityQueue<>((a, b) -> a.distance - b.distance);
        boolean[] inMST = new boolean[n]; // Tracks if a point is in the MST

        // Start with the first point (distance is 0 to itself)
        pq.offer(new Point(0, 0));

        int minCost = 0;
        int pointsConnected = 0;
        
        // Prim's algorithm
        while (pointsConnected < n) {
            Point current = pq.poll();
            
            // Skip if the point is already in the MST
            if (inMST[current.index]) {
                continue;
            }
            
            // Add the point to MST
            inMST[current.index] = true;
            minCost += current.distance;
            pointsConnected++;

            // Update the priority queue with distances to the new point in the MST
            for (int i = 0; i < n; i++) {
                if (!inMST[i]) {
                    int distance = Math.abs(points[current.index][0] - points[i][0]) + 
                                  Math.abs(points[current.index][1] - points[i][1]);
                    pq.offer(new Point(i, distance));
                }
            }
        }

        return minCost;
    }

    // Helper class to store point index and its distance
    static class Point {
        int index;
        int distance;

        Point(int index, int distance) {
            this.index = index;
            this.distance = distance;
        }
    }
}
```

## Approach
1. **Prim's Algorithm**:
   - We use Prim's algorithm to find the Minimum Spanning Tree (MST) of the complete graph where each edge weight is the Manhattan distance between points.
   - Start with an arbitrary point (point 0) and add it to the MST.
   - Use a min-heap to always pick the edge with the smallest weight that connects a vertex in the MST to a vertex outside the MST.
   - Continue until all points are included in the MST.

2. **Data Structures**:
   - `PriorityQueue<Point>`: Min-heap to efficiently get the point with the minimum distance.
   - `boolean[] inMST`: Tracks which points are already included in the MST.
   - `Point` class: Stores the index of the point and its distance from the current MST.

3. **Distance Calculation**:
   - The Manhattan distance between two points `(x1, y1)` and `(x2, y2)` is calculated as `|x1 - x2| + |y1 - y2|`.

## Complexity
- **Time Complexity**: O(n² log n)
  - In the worst case, we process each of the n points, and for each point, we perform heap operations that take O(log n) time.
  - The total number of heap operations is O(n²) in the worst case, leading to O(n² log n) time complexity.
- **Space Complexity**: O(n²)
  - The priority queue can store up to O(n²) edges in the worst case.
  - The `inMST` array uses O(n) space.

## Edge Cases
- Single point: `[[0,0]]` → `0` (no connections needed)
- Two points: `[[0,0],[1,1]]` → `2` (direct connection)
- Points in a straight line: `[[0,0],[0,1],[0,2]]` → `2` (0-1 and 1-2)
- Large input size (1000 points): Should complete within time limits

## Key Insights
- The problem reduces to finding the Minimum Spanning Tree (MST) of a complete graph where the edge weights are Manhattan distances.
- Prim's algorithm is suitable because it efficiently finds the MST by always adding the minimum weight edge that connects a vertex in the MST to a vertex outside the MST.
- The use of a min-heap ensures that we always process the smallest available edge weight next, which is crucial for the greedy approach of Prim's algorithm.
- The solution efficiently handles the constraints by leveraging the priority queue to always select the next best edge to add to the MST.