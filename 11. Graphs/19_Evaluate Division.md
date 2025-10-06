# 399. Evaluate Division

## Problem Statement
You are given an array of variable pairs `equations` and an array of real numbers `values`, where `equations[i] = [A_i, B_i]` and `values[i]` represent the equation `A_i / B_i = values[i]`. Each `A_i` or `B_i` is a string that represents a single variable.

You are also given some `queries`, where `queries[j] = [C_j, D_j]` represents the `jth` query where you must find the answer for `C_j / D_j = ?`.

Return the answers to all queries. If a single answer cannot be determined, return `-1.0`.

### Examples

**Example 1:**
```
Input: equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
Output: [6.00000,0.50000,-1.00000,1.00000,-1.00000]
Explanation: 
Given: a / b = 2.0, b / c = 3.0
queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
return: [6.0, 0.5, -1.0, 1.0, -1.0]
```

**Example 2:**
```
Input: equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
Output: [3.75000,0.40000,5.00000,0.20000]
```

**Example 3:**
```
Input: equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
Output: [0.50000,2.00000,-1.00000,-1.00000]
```

### Constraints:
- `1 <= equations.length <= 20`
- `equations[i].length == 2`
- `1 <= A_i.length, B_i.length <= 5`
- `values.length == equations.length`
- `0.0 < values[i] <= 20.0`
- `1 <= queries.length <= 20`
- `queries[i].length == 2`
- `1 <= C_j.length, D_j.length <= 5`
- `A_i, B_i, C_j, D_j` consist of lower case English letters and digits.

## Solution
```java
import java.util.*;

class Solution {
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
        // Build the graph: source -> {destination -> value}
        Map<String, Map<String, Double>> graph = new HashMap<>();

        // Initialize the graph with direct edges
        for (int i = 0; i < equations.size(); i++) {
            String a = equations.get(i).get(0);
            String b = equations.get(i).get(1);
            double value = values[i];
            
            // Add a -> b with weight value
            graph.computeIfAbsent(a, k -> new HashMap<>()).put(b, value);
            // Add b -> a with weight 1/value
            graph.computeIfAbsent(b, k -> new HashMap<>()).put(a, 1.0 / value);
        }

        // Floyd-Warshall algorithm to compute all possible paths
        for (String k : graph.keySet()) {
            for (String i : graph.get(k).keySet()) {
                for (String j : graph.get(k).keySet()) {
                    // If there's a path i -> k and k -> j, then i -> j = (i -> k) * (k -> j)
                    graph.computeIfAbsent(i, x -> new HashMap<>())
                         .putIfAbsent(j, graph.get(i).get(k) * graph.get(k).get(j));
                }
            }
        }

        // Process each query
        double[] result = new double[queries.size()];
        for (int i = 0; i < queries.size(); i++) {
            String a = queries.get(i).get(0);
            String b = queries.get(i).get(1);
            
            if (graph.containsKey(a) && graph.get(a).containsKey(b)) {
                result[i] = graph.get(a).get(b);
            } else {
                result[i] = -1.0;
            }
        }

        return result;
    }
}
```

## Approach
1. **Graph Construction**:
   - Build a directed weighted graph where nodes are variables and edges represent division results.
   - For each equation `a / b = value`, add edges `a -> b` with weight `value` and `b -> a` with weight `1/value`.

2. **All-Pairs Shortest Path (Floyd-Warshall)**:
   - For each intermediate node `k`, update the weights between all pairs of nodes `(i, j)` using the formula: `i -> j = (i -> k) * (k -> j)`.
   - This computes all possible division results between any two connected variables.

3. **Query Processing**:
   - For each query `[a, b]`, check if there's a direct edge from `a` to `b` in the graph.
   - If found, return the weight; otherwise, return `-1.0`.

## Complexity
- **Time Complexity**: O(n³ + q), where `n` is the number of unique variables and `q` is the number of queries.
  - Building the graph takes O(e) time, where `e` is the number of equations.
  - Floyd-Warshall algorithm takes O(n³) time.
  - Processing queries takes O(q) time.
- **Space Complexity**: O(n²) to store the graph.

## Edge Cases
- Variables not present in any equation.
- Division by zero (handled by problem constraints).
- Variables that are not connected in the graph.
- Self-division (a / a).
- Maximum constraints (20 equations, 20 queries).

## Key Insights
- The problem can be modeled as a graph where edges represent division relationships.
- Floyd-Warshall efficiently computes all possible division results in O(n³) time.
- The solution handles all edge cases and constraints effectively.
- The graph representation allows for O(1) query responses after preprocessing.