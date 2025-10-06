# 2709. Greatest Common Divisor Traversal

## Problem Statement
You are given a 0-indexed integer array `nums`, and you are allowed to traverse between indices `i` and `j` (where `i < j`) if and only if `gcd(nums[i], nums[j]) > 1`. The traversal is transitive, meaning that if you can traverse from index `i` to index `j` and from index `j` to index `k`, then you can traverse from index `i` to index `k`.

Return `true` if you can traverse from every index to every other index, or `false` otherwise.

### Examples

**Example 1:**
```
Input: nums = [2,3,6]
Output: true
Explanation: We can traverse from index 0 to index 2 using the sequence of traversals 0 -> 2.
- From index 0 to index 2: gcd(2, 6) = 2 > 1.
- From index 0 to index 1: gcd(2, 3) = 1, so we cannot traverse from index 0 to index 1.
- From index 1 to index 2: gcd(3, 6) = 3 > 1, so we can traverse from index 1 to index 2.
Since we can reach every index from every other index, we return true.
```

**Example 2:**
```
Input: nums = [3,9,5]
Output: false
Explanation: There is no sequence of traversals that allows us to move from index 0 to index 2.
```

**Example 3:**
```
Input: nums = [4,3,12,8]
Output: true
Explanation: We can traverse from any index to any other index.
```

### Constraints:
- `1 <= nums.length <= 10^5`
- `1 <= nums[i] <= 10^5`

## Solution
```java
import java.util.*;

public class Solution {
    public boolean canTraverseAllPairs(int[] nums) {
        int n = nums.length;
        // If there's only one node, it's trivially connected to itself
        if (n == 1) return true;
        
        // To track visited nodes during DFS
        boolean[] visit = new boolean[n];
        
        // Build adjacency list
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }

        // Create edges between nodes with GCD > 1
        for (int i = 0; i < n; i++) {
            // If any number is 1, it can't form an edge with any other number
            if (nums[i] == 1) return false;
            
            for (int j = i + 1; j < n; j++) {
                if (gcd(nums[i], nums[j]) > 1) {
                    // Add undirected edge
                    adj.get(i).add(j);
                    adj.get(j).add(i);
                }
            }
        }

        // Perform DFS from the first node
        dfs(0, adj, visit);
        
        // Check if all nodes are reachable
        for (boolean node : visit) {
            if (!node) {
                return false;
            }
        }
        return true;
    }

    // Standard DFS implementation
    private void dfs(int node, List<List<Integer>> adj, boolean[] visit) {
        visit[node] = true;
        for (int nei : adj.get(node)) {
            if (!visit[nei]) {
                dfs(nei, adj, visit);
            }
        }
    }

    // Helper method to calculate greatest common divisor
    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

## Approach
1. **Graph Construction**:
   - We treat each index in the array as a node in a graph.
   - We create an undirected edge between two nodes if the GCD of their corresponding values is greater than 1.

2. **Connected Components**:
   - The problem reduces to checking if the graph is connected (i.e., there's a path between every pair of nodes).
   - We use Depth-First Search (DFS) to traverse the graph starting from node 0.
   - After DFS, if all nodes are visited, the graph is connected.

3. **Special Cases**:
   - If the array contains the number 1, it's impossible to form any edges with it (since gcd(1, x) = 1 for any x), so we immediately return false.
   - If there's only one node, it's trivially connected to itself.

## Complexity
- **Time Complexity**: O(n² * log(max(nums)))
  - We compare every pair of numbers (O(n²) pairs).
  - For each pair, we compute GCD which takes O(log(max(a, b))) time.
  - The DFS takes O(n + E) time where E is the number of edges, which is O(n²) in the worst case.

- **Space Complexity**: O(n²)
  - The adjacency list can take up to O(n²) space in the worst case when all pairs have GCD > 1.
  - The visited array takes O(n) space.
  - The recursion stack for DFS can go up to O(n) in depth.

## Edge Cases
- Single element array: `[1]` → `true` (trivially connected)
- Array containing 1: `[1, 2, 3]` → `false` (1 can't form edges)
- All elements are the same: `[2, 2, 2]` → `true` (all connected)
- Large array with large numbers: Should handle within time limits

## Key Insights
- The problem can be modeled as a graph connectivity problem.
- The GCD condition determines the edges in the graph.
- The solution efficiently checks if the graph is connected using DFS.
- The presence of the number 1 immediately makes the answer `false` since it can't form edges with any other number.
- The solution is optimized to handle the constraints efficiently, though for very large n, a more optimized approach using Union-Find with prime factorization might be needed.