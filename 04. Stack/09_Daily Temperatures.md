# 739. Daily Temperatures
**Medium**

## Problem Statement
Given an array of integers `temperatures` represents the daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the `i`th day to get a warmer temperature. If there is no future day for which this is possible, keep `answer[i]` = 0 instead.

## Examples
### Example 1:
- Input: temperatures = [73,74,75,71,69,72,76,73]
- Output: [1,1,4,2,1,1,0,0]

### Example 2:
- Input: temperatures = [30,40,50,60]
- Output: [1,1,1,0]

### Example 3:
- Input: temperatures = [30,60,90]
- Output: [1,1,0]

## Constraints
- 1 <= temperatures.length <= 10^5
- 30 <= temperatures[i] <= 100

## Solution
```java
class Solution {
	public int[] dailyTemperatures(int[] temperatures) {
		int n = temperatures.length;
		int[] answer = new int[n];
		Stack<Integer> stack = new Stack<>();

		for (int i = 0; i < n; i++) {
			while (!stack.isEmpty() && temperatures[i] > temperatures[stack.peek()]) {
				int index = stack.pop();
				answer[index] = i - index;
			}
			stack.push(i);
		}
		return answer;
	}
}
```

Approach

- Use a monotonic decreasing stack to keep track of indices of unresolved temperatures.
- For each day, pop indices from the stack while the current temperature is higher than the temperature at the stack's top index.
- For each popped index, set the answer as the difference between the current index and the popped index.
- Push the current index onto the stack.
- Remaining indices in the stack have no warmer day in the future (answer remains 0).

Time Complexity

O(n), where n is the length of `temperatures` (each index is pushed and popped at most once).

Space Complexity

O(n), for the stack and the answer array.
