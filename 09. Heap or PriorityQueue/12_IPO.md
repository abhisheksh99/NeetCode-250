# IPO

**Difficulty:** Hard

## Problem Statement
Suppose LeetCode will start its IPO soon. In order to sell a good price of its shares to Venture Capital, LeetCode would like to work on some projects to increase its capital before the IPO. You are given `n` projects where the i-th project has a profit `profits[i]` and a minimum capital `capital[i]` is needed to start it.

Initially, you have `w` capital. When you finish a project, you will obtain its profit and the profit will be added to your total capital.

Pick at most `k` distinct projects from given projects to maximize your final capital, and return the final maximized capital.

**Example:**
```
Input: k = 2, w = 0, profits = [1,2,3], capital = [0,1,1]
Output: 4
Explanation: Pick project 0 (profit=1, capital=0), then project 2 (profit=3, capital=1).
```

**Constraints:**
- 1 <= k <= 10^5
- 0 <= w <= 10^9
- n == profits.length == capital.length
- 1 <= n <= 10^5
- 0 <= profits[i], capital[i] <= 10^9

---

## Java Solution
```java
public class Solution {
	public int findMaximizedCapital(int k, int w, int[] profits, int[] capital) {
		PriorityQueue<Integer> minCapital = new PriorityQueue<>((a, b) -> capital[a] - capital[b]);
		PriorityQueue<Integer> maxProfit = new PriorityQueue<>((a, b) -> profits[b] - profits[a]);

		for (int i = 0; i < capital.length; i++) {
			minCapital.offer(i);
		}

		for (int i = 0; i < k; i++) {
			while (!minCapital.isEmpty() && capital[minCapital.peek()] <= w) {
				maxProfit.offer(minCapital.poll());
			}
			if (maxProfit.isEmpty()) {
				break;
			}
			w += profits[maxProfit.poll()];
		}

		return w;
	}
}
```

---

## Approach
- Use a min-heap to keep track of projects by their required capital.
- Use a max-heap to select the most profitable project among those that can be started with the current capital.
- For up to k rounds, move all projects that can be started into the max-heap, then pick the most profitable one.
- Add its profit to the capital and repeat.
- Stop if no more projects can be started.

---

## Time and Space Complexity
- **Time Complexity:** O(n log n + k log n), where n is the number of projects.
- **Space Complexity:** O(n) for the heaps.
