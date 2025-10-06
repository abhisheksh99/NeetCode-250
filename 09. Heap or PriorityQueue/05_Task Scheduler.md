# Task Scheduler

**Difficulty:** Medium

## Problem Statement
Given a characters array `tasks`, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU can complete either one task or be idle.

However, there is a non-negative integer `n` that represents the cooldown period between two same tasks (the same task must be separated by at least n units of time).

Return the least number of units of times that the CPU will take to finish all the given tasks.

**Example:**
```
Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
Explanation: A -> B -> idle -> A -> B -> idle -> A -> B
```

**Constraints:**
- 1 <= tasks.length <= 10^4
- tasks[i] is an uppercase English letter.
- 0 <= n <= 100

---

## Java Solution
```java
class Solution {
	public int leastInterval(char[] tasks, int n) {
		// Step 1: Count the frequency of each task
		Map<Character, Integer> freqMap = new HashMap<>();
		for (char task : tasks) {
			freqMap.put(task, freqMap.getOrDefault(task, 0) + 1);
		}

		// Step 2: Build a max heap based on the frequency
		PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
		maxHeap.addAll(freqMap.values());

		// Step 3: Process tasks
		int time = 0;
		while (!maxHeap.isEmpty()) {
			List<Integer> temp = new ArrayList<>();
			for (int i = 0; i < n + 1; i++) {
				if (!maxHeap.isEmpty()) {
					temp.add(maxHeap.poll());
				}
			}

			for (int freq : temp) {
				if (--freq > 0) {
					maxHeap.add(freq);
				}
			}

			// Step 4: Update time
			time += maxHeap.isEmpty() ? temp.size() : n + 1;
		}

		return time;
	}
}
```

---

## Approach
- Count the frequency of each task using a HashMap.
- Use a max-heap to always schedule the most frequent remaining task.
- In each round, try to execute up to n+1 tasks (to respect the cooldown).
- After each round, re-add tasks with remaining counts to the heap.
- If the heap is empty, add only the number of tasks processed; otherwise, add n+1 to the time.

---

## Time and Space Complexity
- **Time Complexity:** O(N log 26) = O(N), where N is the number of tasks (since there are at most 26 different tasks).
- **Space Complexity:** O(1) (since the heap and map size are bounded by 26).
