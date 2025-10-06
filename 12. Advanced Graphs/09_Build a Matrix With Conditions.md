# 2392. Build a Matrix With Conditions

## Problem Statement
You are given a positive integer `k`. You are also given two 2D integer arrays `rowConditions` and `colConditions` where:

- `rowConditions[i] = [above_i, below_i]` indicates that the number `above_i` should appear in a row that is above the row of `below_i` in the matrix.
- `colConditions[i] = [left_i, right_i]` indicates that the number `left_i` should appear in a column that is to the left of the column of `right_i` in the matrix.

You need to construct a `k x k` matrix that satisfies both the `rowConditions` and `colConditions`. The answer should be a 2D array of size `k x k` where the cell at position `(i, j)` contains the number that should be placed there. If there are multiple valid answers, return any of them. If no valid matrix exists, return an empty 2D array.

### Examples

**Example 1:**
```
Input: k = 3, rowConditions = [[1,2],[3,2]], colConditions = [[2,1]]
Output: [[3,0,0],[0,0,1],[0,2,0]]
Explanation: The diagram above shows a valid example of a matrix that satisfies all the conditions.
The row conditions are the following:
- Number 1 is in row 1, and number 2 is in row 2, so 1 is above 2 in the matrix.
- Number 3 is in row 0, and number 2 is in row 2, so 3 is above 2 in the matrix.
The column conditions are the following:
- Number 2 is at column 1, and number 1 is at column 2, so 2 is to the left of 1 in the matrix.
Note that there may be multiple correct answers.
```

**Example 2:**
```
Input: k = 3, rowConditions = [[1,2],[2,3],[3,1],[2,3]], colConditions = [[2,1]]
Output: []
Explanation: From the first three conditions, 3 has to be in a row that is below 2, but from the fourth condition, 3 has to be in a row that is above 2, which is impossible. Therefore, no valid matrix exists.
```

### Constraints:
- `2 <= k <= 400`
- `1 <= rowConditions.length, colConditions.length <= 10^4`
- `rowConditions[i].length == colConditions[i].length == 2`
- `1 <= above_i, below_i, left_i, right_i <= k`
- `above_i != below_i`
- `left_i != right_i`
- All pairs `(above_i, below_i)` are distinct.
- All pairs `(left_i, right_i)` are distinct.

## Solution
```java
import java.util.*;

class Solution {
    private int k;

    // Topological sort function
    private int[] topologicalSort(int[][] conditions) {
        // Build the graph
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i <= k; i++) {
            graph.add(new ArrayList<>());
        }

        // Calculate in-degree and build adjacency list
        int[] indegree = new int[k + 1];
        for (int[] cond : conditions) {
            int from = cond[0], to = cond[1];
            graph.get(from).add(to);
            indegree[to]++;
        }

        // Initialize queue with nodes having 0 in-degree
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 1; i <= k; i++) {
            if (indegree[i] == 0) queue.offer(i);
        }

        // Perform topological sort
        int[] order = new int[k];
        int index = 0;

        while (!queue.isEmpty()) {
            int curr = queue.poll();
            order[index++] = curr;

            // Reduce in-degree of neighbors
            for (int nei : graph.get(curr)) {
                indegree[nei]--;
                if (indegree[nei] == 0) queue.offer(nei);
            }
        }

        // Check for cycles
        if (index != k) return new int[0];
        return order;
    }

    public int[][] buildMatrix(int k, int[][] rowConditions, int[][] colConditions) {
        this.k = k;
        
        // Get row and column orders using topological sort
        int[] rowOrder = topologicalSort(rowConditions);
        int[] colOrder = topologicalSort(colConditions);

        // If either sort has a cycle, return empty matrix
        if (rowOrder.length == 0 || colOrder.length == 0) return new int[0][0];

        // Create position mapping for columns
        int[] colPos = new int[k + 1];
        for (int i = 0; i < k; i++) {
            colPos[colOrder[i]] = i;
        }

        // Build the result matrix
        int[][] matrix = new int[k][k];
        for (int i = 0; i < k; i++) {
            int num = rowOrder[i];
            matrix[i][colPos[num]] = num;
        }
        
        return matrix;
    }
}
```

## Approach
1. **Topological Sorting**:
   - We use topological sort to determine the relative ordering of numbers in rows and columns based on the given conditions.
   - The `topologicalSort` function builds a graph from the conditions and performs a standard topological sort using Kahn's algorithm.

2. **Cycle Detection**:
   - If the topological sort completes without visiting all nodes, it means there's a cycle in the graph, making the conditions impossible to satisfy.
   - In such cases, we return an empty matrix.

3. **Matrix Construction**:
   - We create a position mapping for columns based on the column order from the topological sort.
   - Then, we place each number in the matrix according to its row order and column position.

## Complexity
- **Time Complexity**: O(k + E)
  - Where E is the total number of conditions (edges in the graph).
  - Building the graph and calculating in-degrees takes O(k + E) time.
  - The topological sort also takes O(k + E) time.
- **Space Complexity**: O(k + E)
  - We store the graph, in-degree array, and other auxiliary data structures.
  - The output matrix takes O(k²) space, but this is part of the output and not counted in the space complexity.

## Edge Cases
- No valid matrix exists due to conflicting conditions (cycles in the graph).
- Multiple valid matrices exist (the problem allows returning any of them).
- Large k (up to 400) with many conditions.
- Empty conditions (all numbers can be placed in any order).

## Key Insights
- The problem can be reduced to finding a valid topological order for both row and column constraints.
- We need to ensure both row and column conditions are satisfied simultaneously.
- The solution efficiently combines two topological sorts to satisfy both sets of constraints.
- The use of Kahn's algorithm allows for efficient cycle detection during the topological sort.