# 1462. Course Schedule IV

## Problem Statement
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [a_i, b_i]` indicates that you must take course `b_i` first if you want to take course `a_i`.

You are also given an array `queries` where `queries[j] = [u_j, v_j]`. For the jth query, determine if course `u_j` is a prerequisite of course `v_j`.

Return a boolean array `answer`, where `answer[j]` is the answer to the jth query.

### Examples

**Example 1:**
```
Input: numCourses = 2, prerequisites = [[1,0]], queries = [[0,1],[1,0]]
Output: [false,true]
Explanation: The pair [1, 0] indicates that you have to take course 1 before you can take course 0.
Course 0 is not a prerequisite of course 1, but the opposite is true.
```

**Example 2:**
```
Input: numCourses = 2, prerequisites = [], queries = [[1,0],[0,1]]
Output: [false,false]
Explanation: There are no prerequisites, so no course is a prerequisite of any other.
```

**Example 3:**
```
Input: numCourses = 3, prerequisites = [[1,2],[1,0],[2,0]], queries = [[1,0],[1,2]]
Output: [true,true]
```

### Constraints:
- `2 <= numCourses <= 100`
- `0 <= prerequisites.length <= (numCourses * (numCourses - 1) / 2)`
- `prerequisites[i].length == 2`
- `0 <= a_i, b_i <= numCourses - 1`
- `a_i != b_i`
- All pairs `[a_i, b_i]` are unique.
- The prerequisites graph has no cycles.
- `1 <= queries.length <= 10^4`
- `0 <= u_i, v_i <= numCourses - 1`
- `u_i != v_i`

## Solution
```java
import java.util.*;

class Solution {
    public List<Boolean> checkIfPrerequisite(int numCourses, int[][] prerequisites, int[][] queries) {
        // Create a 2D boolean array to track if i is a prerequisite of j
        boolean[][] isPrerequisite = new boolean[numCourses][numCourses];

        // Mark direct prerequisites
        for (int[] prereq : prerequisites) {
            int course = prereq[0];
            int required = prereq[1];
            isPrerequisite[course][required] = true;
        }

        // Floyd-Warshall algorithm to find all possible prerequisites (transitive closure)
        for (int k = 0; k < numCourses; k++) {
            for (int i = 0; i < numCourses; i++) {
                for (int j = 0; j < numCourses; j++) {
                    // If i is a prerequisite of k and k is a prerequisite of j,
                    // then i is also a prerequisite of j
                    if (isPrerequisite[i][k] && isPrerequisite[k][j]) {
                        isPrerequisite[i][j] = true;
                    }
                }
            }
        }

        // Process each query
        List<Boolean> result = new ArrayList<>();
        for (int[] query : queries) {
            int u = query[0];
            int v = query[1];
            result.add(isPrerequisite[u][v]);
        }
        
        return result;
    }
}
```

## Approach
1. **Graph Representation**:
   - Use a 2D boolean array `isPrerequisite` where `isPrerequisite[i][j]` is `true` if course `i` is a prerequisite for course `j`.

2. **Direct Prerequisites**:
   - Initialize the `isPrerequisite` array with direct prerequisites from the input.

3. **Transitive Closure with Floyd-Warshall**:
   - For each intermediate course `k`, update the prerequisite relationships to include indirect prerequisites.
   - If `i` is a prerequisite of `k` and `k` is a prerequisite of `j`, then `i` is also a prerequisite of `j`.

4. **Query Processing**:
   - For each query `[u, v]`, check if `u` is a prerequisite of `v` using the computed `isPrerequisite` array.

## Complexity
- **Time Complexity**: O(n³ + q), where `n` is the number of courses and `q` is the number of queries.
  - Floyd-Warshall algorithm takes O(n³) time.
  - Processing queries takes O(q) time.
- **Space Complexity**: O(n²) for the `isPrerequisite` 2D array.

## Edge Cases
- No prerequisites (empty `prerequisites` array).
- Single course (though constraints require at least 2).
- Multiple prerequisites forming a chain (A→B→C).
- Multiple independent prerequisite chains.
- Maximum constraints (100 courses, 4950 prerequisites, 10000 queries).

## Key Insights
- The problem reduces to finding the transitive closure of the prerequisite graph.
- Floyd-Warshall is efficient for small graphs (n ≤ 100) and handles all-pairs shortest paths or reachability.
- The solution efficiently preprocesses all possible prerequisite relationships to answer each query in O(1) time.
- The constraints ensure that the O(n³) Floyd-Warshall algorithm is feasible.