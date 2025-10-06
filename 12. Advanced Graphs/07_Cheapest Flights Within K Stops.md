# 787. Cheapest Flights Within K Stops

## Problem Statement
There are `n` cities connected by some number of flights. You are given an array `flights` where `flights[i] = [from_i, to_i, price_i]` indicates that there is a flight from city `from_i` to city `to_i` with cost `price_i`.

You are also given three integers `src`, `dst`, and `k`, return the cheapest price from `src` to `dst` with at most `k` stops. If there is no such route, return `-1`.

### Examples

**Example 1:**
```
Input: n = 4, flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]], src = 0, dst = 3, k = 1
Output: 700
Explanation:
The optimal path with at most 1 stop from city 0 to 3 is marked in red and has cost 100 + 600 = 700.
Note that the path through cities [0,1,2,3] is cheaper but is invalid because it uses 2 stops.
```

**Example 2:**
```
Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
Output: 200
Explanation:
The cheapest price from city 0 to city 2 with at most 1 stop costs 200.
```

### Constraints:
- `1 <= n <= 100`
- `0 <= flights.length <= (n * (n - 1) / 2)`
- `flights[i].length == 3`
- `0 <= from_i, to_i < n`
- `from_i != to_i`
- `1 <= price_i <= 10^4`
- There will not be any multiple flights between two cities.
- `0 <= src, dst < n`
- `src != dst`
- `0 <= k < n`

## Solution
```java
import java.util.*;

class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        // Initialize costs array with infinity
        int[] costs = new int[n];
        Arrays.fill(costs, Integer.MAX_VALUE);
        costs[src] = 0;  // Cost to reach source is 0

        // Relax edges up to k+1 times (k stops means k+1 edges)
        for (int i = 0; i <= k; i++) {
            // Create a copy of current costs for this iteration
            int[] temp = costs.clone();
            
            // Relax all edges
            for (int[] flight : flights) {
                int u = flight[0], v = flight[1], cost = flight[2];
                
                // Skip if the source node u is not reachable yet
                if (costs[u] == Integer.MAX_VALUE) continue;
                
                // If we found a cheaper path to v through u
                if (temp[v] > costs[u] + cost) {
                    temp[v] = costs[u] + cost;
                }
            }
            
            // Update costs for the next iteration
            costs = temp;
        }
        
        // Return the cost to reach destination or -1 if not reachable
        return costs[dst] == Integer.MAX_VALUE ? -1 : costs[dst];
    }
}
```

## Approach
1. **Bellman-Ford Algorithm**:
   - We use a modified Bellman-Ford algorithm that limits the number of relaxations to `k+1` (since `k` stops mean at most `k+1` edges).
   - This approach efficiently handles the constraint of at most `k` stops.

2. **Relaxation Process**:
   - We maintain a `costs` array where `costs[i]` represents the minimum cost to reach city `i`.
   - In each iteration, we relax all edges (flights) and update the costs if we find a cheaper path.
   - We use a temporary array to ensure updates in the current iteration don't affect other updates in the same iteration.

3. **Termination**:
   - After `k+1` iterations, we have the minimum cost to reach each city with at most `k` stops.
   - We return the cost to reach the destination or `-1` if it's not reachable within the given constraints.

## Complexity
- **Time Complexity**: O(k * E)
  - Where `E` is the number of flights (edges).
  - We perform `k+1` iterations, and in each iteration, we process all `E` edges.
- **Space Complexity**: O(n)
  - We use two arrays of size `n` to store the current and previous costs.
  - The space used does not depend on the number of edges, making it efficient for large graphs.

## Edge Cases
- Direct flight available: `n=3, flights=[[0,1,100],[1,2,100],[0,2,500]], src=0, dst=2, k=0` → `500`
- No path exists: `n=3, flights=[[0,1,100]], src=0, dst=2, k=1` → `-1`
- Multiple paths with different stops: `n=4, flights=[[0,1,1],[1,2,1],[2,3,1],[0,3,5]], src=0, dst=3, k=1` → `5`
- Large number of cities and flights: Should complete within time limits

## Key Insights
- The problem can be efficiently solved using a modified Bellman-Ford algorithm that limits the number of relaxations.
- The use of a temporary array ensures that updates in the current iteration don't affect other updates in the same iteration.
- The solution efficiently handles the constraint of at most `k` stops by limiting the number of iterations to `k+1`.
- The space complexity is optimized by reusing arrays and not storing the entire graph explicitly.