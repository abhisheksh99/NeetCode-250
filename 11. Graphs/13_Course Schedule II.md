# 210. Course Schedule II

## Problem Statement
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [a_i, b_i]` indicates that you must take course `b_i` first if you want to take course `a_i`.

- For example, the pair `[0, 1]` indicates that to take course `0` you have to first take course `1`.

Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.

### Examples

**Example 1:**
```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So the correct course order is [0,1].
```

**Example 2:**
```
Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
Explanation: There are a total of 4 courses to take. 
To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
```

**Example 3:**
```
Input: numCourses = 1, prerequisites = []
Output: [0]
```

### Constraints:
- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= numCourses * (numCourses - 1)`
- `prerequisites[i].length == 2`
- `0 <= a_i, b_i < numCourses`
- `a_i != b_i`
- All the pairs `[a_i, b_i]` are **unique**.

## Solution
```java
import java.util.*;

class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        // Initialize the graph as an adjacency list
        List<Integer>[] graph = new ArrayList[numCourses];
        // Initialize in-degree array to count incoming edges
        int[] inDegree = new int[numCourses];
        
        // Initialize each course with an empty list of neighbors
        for (int i = 0; i < numCourses; i++) {
            graph[i] = new ArrayList<>();
        }
        
        // Build the graph and calculate in-degrees
        for (int[] pre : prerequisites) {
            int course = pre[0];
            int prereq = pre[1];
            graph[prereq].add(course);  // Add edge from prereq to course
            inDegree[course]++;          // Increment in-degree of the course
        }
        
        // Queue for BFS - start with courses that have no prerequisites
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i);
            }
        }
        
        // Array to store the topological order
        int[] order = new int[numCourses];
        int index = 0;  // Index to track position in the order array
        
        // Process the graph using BFS
        while (!queue.isEmpty()) {
            int course = queue.poll();
            order[index++] = course;  // Add course to the order
            
            // For each neighbor, decrement in-degree and add to queue if in-degree becomes 0
            for (int nextCourse : graph[course]) {
                inDegree[nextCourse]--;
                if (inDegree[nextCourse] == 0) {
                    queue.offer(nextCourse);
                }
            }
        }
        
        // If we've processed all courses, return the order; otherwise, return empty array
        return (index == numCourses) ? order : new int[0];
    }
}
```

## Approach
1. **Graph Representation**:
   - Represent the courses and their prerequisites as a directed graph using an adjacency list.
   - Each course is a node, and a directed edge from A to B means A is a prerequisite for B.

2. **Kahn's Algorithm for Topological Sort**:
   - Calculate the in-degree (number of incoming edges) for each node.
   - Initialize a queue with all nodes that have no incoming edges (in-degree = 0).
   - While the queue is not empty:
     - Remove a node from the queue and add it to the topological order.
     - For each neighbor of the node, decrement its in-degree.
     - If a neighbor's in-degree becomes 0, add it to the queue.
   - If the topological order contains all nodes, return it; otherwise, return an empty array (cycle detected).

## Complexity
- **Time Complexity**: O(V + E), where V is the number of courses and E is the number of dependencies.
  - We visit each course and each edge exactly once.
- **Space Complexity**: O(V + E) for the graph representation and O(V) for the queue and in-degree array.

## Edge Cases
- No prerequisites (empty prerequisites array).
- A single course with no prerequisites.
- Courses that form a cycle (should return empty array).
- Maximum constraints (2000 courses with maximum dependencies).
- Multiple valid topological orders (any valid order is acceptable).

## Key Insights
- The problem reduces to finding a topological sort of a directed acyclic graph (DAG).
- If a valid topological sort exists, it's possible to finish all courses; otherwise, there's a cycle.
- Kahn's algorithm is efficient for topological sorting and cycle detection in a DAG.
- The algorithm processes nodes level by level, always selecting nodes with no remaining dependencies first.
- The solution handles all edge cases and constraints efficiently.