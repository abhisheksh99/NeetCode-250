# 207. Course Schedule

## Problem Statement
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [a_i, b_i]` indicates that you must take course `b_i` first if you want to take course `a_i`.

- For example, the pair `[0, 1]` indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

### Examples

**Example 1:**
```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
```

**Example 2:**
```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```

### Constraints:
- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= 5000`
- `prerequisites[i].length == 2`
- `0 <= a_i, b_i < numCourses`
- All the pairs `[a_i, b_i]` are **distinct**.

## Solution
```java
import java.util.*;

class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // Build the graph using adjacency list
        HashMap<Integer, List<Integer>> courseGraph = new HashMap<>();
        
        // Initialize the graph with empty lists
        for (int i = 0; i < numCourses; i++) {
            courseGraph.put(i, new LinkedList<>());
        }
        
        // Add edges to the graph
        for (int[] pre : prerequisites) {
            int course = pre[0];
            int prereq = pre[1];
            courseGraph.get(prereq).add(course);
        }
        
        // Set to track visited nodes in the current DFS path
        HashSet<Integer> visited = new HashSet<>();
        
        // Check each course for cycles
        for (int currentCourse = 0; currentCourse < numCourses; currentCourse++) {
            if (hasCycle(currentCourse, visited, courseGraph)) {
                return false; // Cycle detected, cannot finish all courses
            }
        }
        
        return true; // No cycles detected, can finish all courses
    }
    
    // Returns true if a cycle is detected starting from the given course
    private boolean hasCycle(int course, HashSet<Integer> visited, 
                           HashMap<Integer, List<Integer>> courseGraph) {
        // If we've already visited this node in the current path, cycle detected
        if (visited.contains(course)) {
            return true;
        }
        
        // If this course has no prerequisites, no cycle can be formed from here
        if (courseGraph.get(course).isEmpty()) {
            return false;
        }
        
        // Mark the current course as visited
        visited.add(course);
        
        // Check all next courses in the graph
        for (int nextCourse : courseGraph.get(course)) {
            if (hasCycle(nextCourse, visited, courseGraph)) {
                return true; // Cycle detected in the subtree
            }
        }
        
        // Backtrack: remove the current course from the visited set
        visited.remove(course);
        
        // Optimization: mark this course as processed to avoid redundant work
        courseGraph.put(course, new LinkedList<>());
        
        return false; // No cycle found in this path
    }
}
```

## Approach
1. **Graph Representation**:
   - Represent the courses and their prerequisites as a directed graph using an adjacency list.
   - Each course is a node, and a directed edge from A to B means A is a prerequisite for B.

2. **Cycle Detection**:
   - Use Depth-First Search (DFS) to detect cycles in the graph.
   - A cycle in the graph means it's impossible to finish all courses.
   - Use a `visited` set to track nodes in the current DFS path to detect back edges.

3. **Optimization**:
   - Memoize the results of processed nodes to avoid redundant work.
   - Once a node is processed and found to be cycle-free, mark it as such to prevent reprocessing.

## Complexity
- **Time Complexity**: O(V + E), where V is the number of courses and E is the number of dependencies.
  - We visit each course and each dependency exactly once.
- **Space Complexity**: O(V + E) for the graph representation and O(V) for the recursion stack in the worst case.

## Edge Cases
- No prerequisites (empty prerequisites array).
- A single course with no prerequisites.
- Courses that form a cycle.
- Disconnected graph with multiple components.
- Maximum constraints (2000 courses, 5000 prerequisites).

## Key Insights
- The problem reduces to detecting cycles in a directed graph.
- If there's a cycle in the graph, it's impossible to finish all courses.
- DFS is an efficient way to detect cycles in a directed graph.
- Backtracking is used to explore all possible paths while keeping track of the current path.
- Memoization (marking processed nodes) optimizes the solution by avoiding redundant work.