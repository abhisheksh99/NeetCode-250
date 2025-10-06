# 1489. Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree

## Problem Statement
Given a weighted undirected connected graph with `n` vertices numbered from `0` to `n - 1`, and an array `edges` where `edges[i] = [u_i, v_i, weight_i]` represents a bidirectional and weighted edge between nodes `u_i` and `v_i`, find all the critical and pseudo-critical edges in the graph's minimum spanning tree (MST).

A critical edge is an edge that, if removed, will cause the MST weight to increase or make the graph disconnected. A pseudo-critical edge is an edge that can appear in some MSTs but not all. For each edge, you need to determine if it is either critical, pseudo-critical, or none of them.

Return a list of two lists: the first list should contain the indices of all critical edges, and the second list should contain the indices of all pseudo-critical edges in any order.

### Examples

**Example 1:**
```
Input: n = 5, edges = [[0,1,1],[1,2,1],[2,3,2],[0,3,2],[0,4,3],[3,4,3],[1,4,6]]
Output: [[0,1],[2,3,4,5]]
Explanation: The figure describes the graph.
The following edges are critical:
- 0 (between nodes 0 and 1)
- 1 (between nodes 1 and 2)
The following edges are pseudo-critical:
- 2 (between nodes 2 and 3)
- 3 (between nodes 0 and 3)
- 4 (between nodes 0 and 4)
- 5 (between nodes 3 and 4)
```

**Example 2:**
```
Input: n = 4, edges = [[0,1,1],[1,2,1],[2,3,1],[0,3,1]]
Output: [[],[0,1,2,3]]
Explanation: All edges are pseudo-critical.
```

### Constraints:
- `2 <= n <= 100`
- `1 <= edges.length <= min(200, n * (n - 1) / 2)`
- `edges[i].length == 3`
- `0 <= u_i < v_i < n`
- `1 <= weight_i <= 1000`
- All pairs `(u_i, v_i)` are distinct.

## Solution
```java
import java.util.*;

class Solution {
    // Disjoint Set Union (DSU) data structure
    class DisjointSet {
        int[] parent;
        int[] rank;
        
        DisjointSet(int n) {
            parent = new int[n];
            rank = new int[n];
            for (int i = 0; i < n; i++) parent[i] = i;
        }

        // Path compression
        int find(int x) {
            if (parent[x] != x) parent[x] = find(parent[x]);
            return parent[x];
        }

        // Union by rank
        boolean union(int a, int b) {
            int rootA = find(a);
            int rootB = find(b);
            if (rootA == rootB) return false; // Already in same set
            
            if (rank[rootA] < rank[rootB]) {
                parent[rootA] = rootB;
            } else if (rank[rootB] < rank[rootA]) {
                parent[rootB] = rootA;
            } else {
                parent[rootB] = rootA;
                rank[rootA]++;
            }
            return true;
        }
    }

    public List<List<Integer>> findCriticalAndPseudoCriticalEdges(int n, int[][] edges) {
        int m = edges.length;
        // Create new edges array with original indices
        int[][] newEdges = new int[m][4]; // [u, v, weight, originalIndex]
        for (int i = 0; i < m; ++i) {
            newEdges[i][0] = edges[i][0];
            newEdges[i][1] = edges[i][1];
            newEdges[i][2] = edges[i][2];
            newEdges[i][3] = i; // Store original index
        }

        // Sort edges by weight
        Arrays.sort(newEdges, Comparator.comparingInt(a -> a[2]));
        
        // Find original MST weight
        int originalWeight = mst(n, newEdges, -1, -1);

        List<Integer> critical = new ArrayList<>();
        List<Integer> pseudoCritical = new ArrayList<>();

        // Check each edge
        for (int i = 0; i < m; i++) {
            // Check if edge is critical: exclude it and see if MST weight increases or becomes disconnected
            if (mst(n, newEdges, i, -1) > originalWeight) {
                critical.add(newEdges[i][3]);
            } 
            // Check if edge is pseudo-critical: force include edge and see if MST weight remains same
            else if (mst(n, newEdges, -1, i) == originalWeight) {
                pseudoCritical.add(newEdges[i][3]);
            }
        }
        
        return Arrays.asList(critical, pseudoCritical);
    }

    // Calculate MST weight with optional exclude and include edges
    private int mst(int n, int[][] edges, int excludeEdge, int includeEdge) {
        DisjointSet ds = new DisjointSet(n);
        int weight = 0;
        int edgesUsed = 0;

        // If including an edge forcibly, add it first
        if (includeEdge != -1) {
            int[] edge = edges[includeEdge];
            ds.union(edge[0], edge[1]);
            weight += edge[2];
            edgesUsed++;
        }

        // Build MST with Krusky's algorithm
        for (int i = 0; i < edges.length; i++) {
            // Skip excluded edge or already included edge
            if (i == excludeEdge || i == includeEdge) continue;
            
            int[] edge = edges[i];
            if (ds.union(edge[0], edge[1])) {
                weight += edge[2];
                edgesUsed++;
            }
        }

        // Check if all nodes are connected
        int root = ds.find(0);
        for (int i = 1; i < n; i++) {
            if (ds.find(i) != root) return Integer.MAX_VALUE;
        }
        
        return weight;
    }
}
```

## Approach
1. **Disjoint Set Union (DSU) Data Structure**:
   - Used to efficiently manage and merge sets of nodes.
   - Implements path compression and union by rank for optimal performance.

2. **Finding Original MST Weight**:
   - Sort all edges by weight.
   - Use Krusky's algorithm to find the weight of the original MST.

3. **Identifying Critical Edges**:
   - An edge is critical if removing it increases the MST weight or disconnects the graph.
   - Check this by excluding the edge and recalculating the MST weight.

4. **Identifying Pseudo-Critical Edges**:
   - An edge is pseudo-critical if it can be part of some MST but is not critical.
   - Check this by forcing the edge to be included and verifying if the MST weight remains the same.

## Complexity
- **Time Complexity**: O(E² α(n))
  - Where E is the number of edges and α is the inverse Ackermann function (nearly constant).
  - For each edge, we may perform a full MST calculation which is O(E α(n)).
- **Space Complexity**: O(n + E)
  - O(n) for the DSU data structure.
  - O(E) for storing the edges with their original indices.

## Edge Cases
- Small graph with 2 nodes and 1 edge → The single edge is critical.
- Complete graph where all edges have the same weight → All edges are pseudo-critical.
- Graph with multiple components → No MST exists, but the problem guarantees the graph is connected.
- Graph with all edges having unique weights → Each edge is either critical or not in any MST.

## Key Insights
- The problem requires understanding of MST properties and the Krusky's algorithm.
- Critical edges are those that must be included in every MST.
- Pseudo-critical edges appear in some MSTs but not all.
- The solution efficiently checks each edge's role by recalculating the MST with and without it.
- The DSU data structure is crucial for efficiently managing the connected components during MST construction.
