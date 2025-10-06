# 84. Largest Rectangle in Histogram
**Hard**

## Problem Statement
Given an array of integers `heights` representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.

## Examples
### Example 1:
- Input: heights = [2,1,5,6,2,3]
- Output: 10
- Explanation: The largest rectangle has area 10 (between indices 2 and 3).

### Example 2:
- Input: heights = [2,4]
- Output: 4

## Constraints
- 1 <= heights.length <= 10^5
- 0 <= heights[i] <= 10^4

## Solution
```java
class Solution {
	public int largestRectangleArea(int[] heights) {
		int maxArea = 0;
		Stack<Integer> stack = new Stack<>();
		int n = heights.length;
        
		for (int i = 0; i <= n; i++) {
			int currentHeight = (i == n) ? 0 : heights[i];
			while (!stack.isEmpty() && currentHeight < heights[stack.peek()]) {
				int height = heights[stack.pop()];
				int width = stack.isEmpty() ? i : i - stack.peek() - 1;
				maxArea = Math.max(maxArea, height * width);
			}
			stack.push(i);
		}
        
		return maxArea;
	}
}
```

Approach

- Use a monotonic increasing stack to keep track of indices of bars.
- For each bar, pop from the stack while the current bar is lower than the bar at the stack's top.
- For each popped bar, calculate the area with the popped height as the smallest bar.
- Push the current index onto the stack.
- Add a zero-height bar at the end to ensure all bars are processed.

Time Complexity

O(n), where n is the length of `heights` (each index is pushed and popped at most once).

Space Complexity

O(n), for the stack.
