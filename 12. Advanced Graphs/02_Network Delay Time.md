# 743. Network Delay Time

## Problem Statement
You are given a network of `n` nodes, labeled from `1` to `n`. You are also given `times`, a list of travel times as directed edges `times[i] = (u_i, v_i, w_i)`, where `u_i` is the source node, `v_i` is the target node, and `w_i` is the time it takes for a signal to travel from source to target.

We will send a signal from a given node `k`. Return the minimum time it takes for all the `n` nodes to receive the signal. If it is impossible for all the `n` nodes to receive the signal, return `-1`.

### Examples

**Example 1:**
```
Input: times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
Output: 2
Explanation: The signal is sent from node 2. At time 1, nodes 1 and 3 receive the signal. At time 2, node 4 receives the signal.
```

**Example 2:**
```
Input: times = [[1,2,1]], n = 2, k = 1
Output: 1
```

### Constraints:
- `1 <= k <= n <= 100`
- `1 <= times.length <= 6000`
- `times[i].length == 3`
- `1 <= u_i, v_i <= n`
- `u_i != v_i`
- `0 <= w_i <= 100`
- All the pairs `(u_i, v_i)` are unique. (i.e., no multiple edges)

## Solution
```java
import java.util.*;

class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        // Step 1: Build adjacency list for the graph
        Map<Integer, List<int[]>> graph = new HashMap<>();
        for (int[] edge : times) {
            int source = edge[0];
            int target = edge[1];
            int weight = edge[2];
            graph.computeIfAbsent(source, x -> new ArrayList<>()).add(new int[]{target, weight});
        }

        // Step 2: Min-heap priority queue to always select node with shortest distance
        PriorityQueue<int[]> minHeap = new PriorityQueue<>(Comparator.comparingInt(a -> a[1]));
        minHeap.offer(new int[]{k, 0}); // {node, distance}

        // Step 3: Distance array initialized to "infinity"
        int[] distance = new int[n + 1];
        Arrays.fill(distance, Integer.MAX_VALUE);
        distance[k] = 0; // Distance to start node is 0

        // Step 4: Dijkstra's algorithm
        while (!minHeap.isEmpty()) {
            int[] current = minHeap.poll();
            int node = current[0];
            int dist = current[1];

            // Skip if we have already found a shorter path
            if (dist > distance[node]) continue;

            // Update distances for neighbors
            if (graph.containsKey(node)) {
                for (int[] neighbor : graph.get(node)) {
                    int nextNode = neighbor[0];
                    int weight = neighbor[1];
                    int newDist = dist + weight;

                    if (newDist < distance[nextNode]) {
                        distance[nextNode] = newDist;
                        minHeap.offer(new int[]{nextNode, newDist});
                    }
                }
            }
        }

        // Step 5: Get the maximum distance from start node to all other nodes
        int maxDistance = 0;
        for (int i = 1; i <= n; i++) {
            if (distance[i] == Integer.MAX_VALUE) return -1; // Unreachable node
            maxDistance = Math.max(maxDistance, distance[i]);
        }

        return maxDistance;
    }
}
```

## Approach
1. **Graph Representation**:
   - We represent the network as a directed weighted graph using an adjacency list.
   - Each node maps to a list of its neighbors and the corresponding edge weights.

2. **Dijkstra's Algorithm**:
   - We use a min-heap to always explore the node with the current shortest distance from the source.
   - We maintain a distance array to keep track of the shortest known distance to each node.
   - For each node, we update the distances to its neighbors if a shorter path is found.

3. **Termination**:
   - The algorithm terminates when there are no more nodes to process in the priority queue.
   - We then find the maximum distance in our distance array, which represents the time taken for the signal to reach all nodes.

## Complexity
- **Time Complexity**: O(E log V)
  - Where E is the number of edges and V is the number of vertices.
  - Each edge is processed once, and each heap operation takes O(log V) time.
- **Space Complexity**: O(V + E)
  - O(V) for the distance array and priority queue.
  - O(E) for the adjacency list.

## Edge Cases
- Single node network (n = 1) - should return 0
- Disconnected graph - should return -1 if any node is unreachable
- Multiple paths to the same node - should choose the path with minimum total weight
- Large input sizes - should handle the maximum constraints efficiently

## Key Insights
- The problem is essentially finding the shortest paths from a single source (node k) to all other nodes in the graph.
- Dijkstra's algorithm is suitable because all edge weights are non-negative.
- The answer is the maximum value in the shortest path distances array, as we need all nodes to receive the signal.
- If any node remains at distance `Integer.MAX_VALUE`, it means it's unreachable, and we should return -1.