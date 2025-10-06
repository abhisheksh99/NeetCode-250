# 323. Number of Connected Components in an Undirected Graph

## Problem Statement
You have a graph of `n` nodes. You are given an integer `n` and an array `edges` where `edges[i] = [a_i, b_i]` indicates that there is an undirected edge between nodes `a_i` and `b_i`.

Return the number of connected components in the graph.

### Examples

**Example 1:**
```
Input: n = 5, edges = [[0,1],[1,2],[3,4]]
Output: 2
Explanation: There are 2 connected components:
1. 0-1-2
2. 3-4
```

**Example 2:**
```
Input: n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]
Output: 1
Explanation: All nodes are connected in a single component:
0-1-2-3-4
```

### Constraints:
- `1 <= n <= 2000`
- `0 <= edges.length <= 5000`
- `edges[i].length == 2`
- `0 <= a_i, b_i < n`
- `a_i != b_i`
- There are no repeated edges.
- No self-loops in the graph.

## Solution
```java
import java.util.*;

class Solution {
    public int countComponents(int n, int[][] edges) {
        int counter = 0;
        // 0: unvisited, 1: visited
        int[] visited = new int[n];
        
        // Build adjacency list
        List<Integer>[] adjList = new ArrayList[n];
        for (int i = 0; i < n; i++) {
            adjList[i] = new ArrayList<>();
        }
        
        // Add edges to adjacency list (undirected graph)
        for (int[] edge : edges) {
            adjList[edge[0]].add(edge[1]);
            adjList[edge[1]].add(edge[0]);
        }
        
        // Count connected components using DFS
        for (int i = 0; i < n; i++) {
            if (visited[i] == 0) {
                counter++;
                dfs(adjList, visited, i);
            }
        }
        
        return counter;
    }
    
    // DFS to mark all connected nodes as visited
    private void dfs(List<Integer>[] adjList, int[] visited, int node) {
        // Mark current node as visited
        visited[node] = 1;
        
        // Visit all unvisited neighbors
        for (int neighbor : adjList[node]) {
            if (visited[neighbor] == 0) {
                dfs(adjList, visited, neighbor);
            }
        }
    }
}
```

## Approach
1. **Graph Representation**:
   - Build an adjacency list to represent the undirected graph.
   - For each edge `[a, b]`, add `b` to `a`'s adjacency list and `a` to `b`'s adjacency list.

2. **DFS Traversal**:
   - Initialize a `visited` array to keep track of visited nodes.
   - For each unvisited node, increment the component counter and perform DFS to mark all reachable nodes as visited.
   - Each DFS call from an unvisited node discovers a new connected component.

3. **Result**:
   - The final value of the counter represents the number of connected components in the graph.

## Complexity
- **Time Complexity**: O(V + E), where V is the number of vertices (n) and E is the number of edges.
  - Building the adjacency list takes O(E) time.
  - DFS visits each node and edge exactly once, taking O(V + E) time.
- **Space Complexity**: O(V + E)
  - O(V) for the `visited` array and recursion stack.
  - O(E) for the adjacency list (since it's an undirected graph, we store each edge twice).

## Edge Cases
- No edges (each node is its own component).
- A single node with no edges.
- A fully connected graph (single component).
- Disconnected graph with multiple components of varying sizes.
- Maximum constraints (2000 nodes, 5000 edges).

## Key Insights
- The problem reduces to counting the number of connected components in an undirected graph.
- DFS is an efficient way to explore connected components.
- The solution handles all edge cases, including graphs with no edges and fully connected graphs.
- The algorithm is optimal with O(V + E) time complexity, which is the best possible for this problem.