# Single-Threaded CPU

**Difficulty:** Medium

## Problem Statement
You are given `n` tasks labeled from 0 to n-1, where each task is represented by `tasks[i] = [enqueueTimei, processingTimei]`.

You have a single-threaded CPU that can process at most one task at a time and will process tasks in the following way:
- If the CPU is idle and there are no available tasks, the CPU remains idle.
- If the CPU is idle and there are available tasks, it will choose the one with the shortest processing time. If multiple tasks have the same processing time, it will choose the task with the smallest index.
- Once a task is started, the CPU will process the entire task without stopping.
- The CPU can start a new task immediately after finishing the previous one.

Return the order in which the CPU will process the tasks.

**Example:**
```
Input: tasks = [[1,2],[2,4],[3,2],[4,1]]
Output: [0,2,3,1]
```

**Constraints:**
- 1 <= tasks.length <= 10^5
- 1 <= enqueueTimei, processingTimei <= 10^9

---

## Java Solution
```java
class Solution {
    public int[] getOrder(int[][] tasks) {
        int n = tasks.length;
        int[] result = new int[n];
        int time = 0, index = 0;
        // Add original index to each task to remember order
        for (int i = 0; i < n; ++i)
            tasks[i] = new int[] {tasks[i][0], tasks[i][1], i};
        Arrays.sort(tasks, (a, b) -> a[0] - b[0]);
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> 
            a[1] != b[1] ? a[1] - b[1] : a[2] - b[2]);
        int i = 0;
        for (int j = 0; j < n; ++j) {
            if (i < n && time < tasks[i][0]) time = tasks[i][0];
            while (i < n && tasks[i][0] <= time) {
                pq.offer(tasks[i++]);
            }
            if (pq.isEmpty()) {
                time = tasks[i][0];
                pq.offer(tasks[i++]);
            }
            int[] current = pq.poll();
            result[index++] = current[2];
            time += current[1];
        }
        return result;
    }
}
```

---

## Approach
- Attach the original index to each task for result ordering.
- Sort tasks by enqueue time.
- Use a min-heap to always pick the available task with the shortest processing time (and smallest index for ties).
- Simulate the CPU's operation, updating the current time and result order as tasks are processed.

---

## Time and Space Complexity
- **Time Complexity:** O(n log n), where n is the number of tasks (for sorting and heap operations).
- **Space Complexity:** O(n) for the heap and result array.
