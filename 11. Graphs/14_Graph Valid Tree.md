# 261. Graph Valid Tree

## Problem Statement
You have a graph of `n` nodes labeled from `0` to `n - 1`. You are given an integer `n` and a list of `edges` where `edges[i] = [a_i, b_i]` indicates that there is an undirected edge between nodes `a_i` and `b_i` in the graph.

Return `true` if the edges of the given graph make up a valid tree, and `false` otherwise.

### Examples

**Example 1:**
```
Input: n = 5, edges = [[0,1],[0,2],[0,3],[1,4]]
Output: true
```

**Example 2:**
```
Input: n = 5, edges = [[0,1],[1,2],[2,3],[1,3],[1,4]]
Output: false
```

### Constraints:
- `1 <= n <= 2000`
- `0 <= edges.length <= 5000`
- `edges[i].length == 2`
- `0 <= a_i, b_i < n`
- `a_i != b_i`
- There are no self-loops or repeated edges.

## Solution
```java
import java.util.*;

class Solution {
    public boolean validTree(int n, int[][] edges) {
        // A valid tree must have exactly n-1 edges
        if (edges.length != n-1) {
            return false;
        }
        
        // Build the adjacency list
        List<List<Integer>> adjacencyList = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adjacencyList.add(new ArrayList<>());
        }
        
        // Add edges to the adjacency list (undirected graph)
        for (int[] edge : edges) {
            adjacencyList.get(edge[0]).add(edge[1]);
            adjacencyList.get(edge[1]).add(edge[0]);
        }
        
        // Stack for DFS
        Stack<Integer> stack = new Stack<>();
        // Set to keep track of visited nodes
        Set<Integer> visited = new HashSet<>();
        
        // Start DFS from node 0
        stack.push(0);
        visited.add(0);
        
        // Perform DFS
        while (!stack.isEmpty()) {
            int node = stack.pop();
            
            // Visit all neighbors
            for (int neighbor : adjacencyList.get(node)) {
                // Skip if already visited
                if (visited.contains(neighbor)) {
                    continue;
                }
                
                // Mark as visited and add to stack
                visited.add(neighbor);
                stack.push(neighbor);
            }
        }
        
        // If we've visited all nodes, it's a valid tree
        return visited.size() == n;
    }
}
```

## Approach
1. **Check Edge Count**: A valid tree with `n` nodes must have exactly `n-1` edges. If not, it's either not connected or has cycles.
2. **Build Adjacency List**: Create an undirected graph representation using an adjacency list.
3. **Depth-First Search (DFS)**:
   - Start DFS from node 0.
   - Keep track of visited nodes to avoid cycles.
   - If we can visit all nodes without encountering a cycle, and the number of edges is `n-1`, it's a valid tree.

## Complexity
- **Time Complexity**: O(V + E), where V is the number of vertices (n) and E is the number of edges.
  - Building the adjacency list takes O(E) time.
  - DFS takes O(V + E) time as it visits each node and edge once.
- **Space Complexity**: O(V + E)
  - O(V) for the visited set and stack.
  - O(E) for the adjacency list (since it's an undirected graph, we store each edge twice).

## Edge Cases
- Single node with no edges (trivially a valid tree).
- Disconnected graph (should return false).
- Graph with cycles (should return false).
- Graph with exactly n-1 edges but still disconnected (should return false).
- Empty graph (n=0, edges=[]), though constraints specify n >= 1.

## Key Insights
- A valid tree is a connected acyclic graph with exactly `n-1` edges.
- The solution first checks the edge count for efficiency before performing DFS.
- DFS helps verify both connectivity and acyclicity in one pass.
- The solution efficiently handles all edge cases and constraints.