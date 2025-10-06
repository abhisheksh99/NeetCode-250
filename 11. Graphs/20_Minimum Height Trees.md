# 310. Minimum Height Trees

## Problem Statement
A tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.

Given a tree of `n` nodes labeled from `0` to `n - 1`, and an array of `n - 1` edges where `edges[i] = [a_i, b_i]` indicates that there is an undirected edge between the two nodes `a_i` and `b_i` in the tree, you can choose any node as the root. When you select a node `x` as the root, the result tree has height `h`. Among all possible rooted trees, those with minimum height (i.e., `min(h)`) are called minimum height trees (MHTs).

Return a list of all MHTs' root labels. You can return the answer in any order.

The height of a rooted tree is the number of edges on the longest downward path between the root and a leaf.

### Examples

**Example 1:**
```
Input: n = 4, edges = [[1,0],[1,2],[1,3]]
Output: [1]
Explanation: As shown, the height of the tree is 1 when the root is the node with label 1 which is the only MHT.
```

**Example 2:**
```
Input: n = 6, edges = [[3,0],[3,1],[3,2],[3,4],[5,4]]
Output: [3,4]
```

### Constraints:
- `1 <= n <= 2 * 10^4`
- `edges.length == n - 1`
- `0 <= a_i, b_i < n`
- `a_i != b_i`
- All the pairs `(a_i, b_i)` are distinct.
- The given input is guaranteed to be a tree and there will be no repeated edges.

## Solution
```java
import java.util.*;

class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        // Handle the case where there's only one node
        if (n == 1) return Collections.singletonList(0);

        // Build the graph as an adjacency list
        List<Set<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) graph.add(new HashSet<>());

        // Track the degree of each node
        int[] degree = new int[n];
        
        // Populate the graph and degree array
        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
            degree[edge[0]]++;
            degree[edge[1]]++;
        }

        // Initialize a queue with all leaf nodes (degree = 1)
        Queue<Integer> leaves = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            if (degree[i] == 1) leaves.offer(i);
        }

        int remainingNodes = n;

        // Process the tree layer by layer, removing leaves until we have 1 or 2 nodes left
        while (remainingNodes > 2) {
            int size = leaves.size();
            remainingNodes -= size;
            
            // Process all current leaves
            for (int i = 0; i < size; i++) {
                int leaf = leaves.poll();
                
                // For each neighbor of the current leaf
                for (int neighbor : graph.get(leaf)) {
                    // Decrement the degree and check if it becomes a new leaf
                    if (--degree[neighbor] == 1) {
                        leaves.offer(neighbor);
                    }
                }
            }
        }

        // The remaining nodes in the queue are the roots of MHTs
        return new ArrayList<>(leaves);
    }
}
```

## Approach
1. **Graph Construction**:
   - Build an adjacency list representation of the tree.
   - Track the degree of each node (number of connections).

2. **Leaf Node Identification**:
   - Start with all leaf nodes (nodes with degree 1) in a queue.

3. **Topological Sorting**:
   - Repeatedly remove leaf nodes layer by layer until 1 or 2 nodes remain.
   - When removing a leaf, decrease the degree of its neighbor.
   - If a neighbor's degree becomes 1, add it to the queue as a new leaf.

4. **Result**:
   - The remaining 1 or 2 nodes are the roots of the minimum height trees.

## Complexity
- **Time Complexity**: O(n), where n is the number of nodes.
  - Building the graph takes O(n) time.
  - The BFS-like processing takes O(n) time as each node is processed exactly once.
- **Space Complexity**: O(n) for the graph and queue storage.

## Edge Cases
- Single node tree (n = 1).
- Linear tree (longest path).
- Star-shaped tree.
- Balanced tree.
- Maximum constraints (n = 20,000).

## Key Insights
- The roots of MHTs are the centroids of the tree.
- A tree can have at most 2 centroids.
- The approach efficiently finds centroids by peeling away layers of leaves.
- The solution handles all edge cases and constraints effectively.