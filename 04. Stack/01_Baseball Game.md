# 682. Baseball Game
**Easy**

## Problem Statement
You are keeping score for a baseball game with strange rules. Each operation is a string representing one of the following:
- An integer x: Record a new score of x.
- "+": Record a new score that is the sum of the previous two scores.
- "D": Record a new score that is double the previous score.
- "C": Invalidate the previous score, removing it from the record.

Return the sum of all the scores on the record after all operations.

## Examples
### Example 1:
- Input: ops = ["5","2","C","D","+"]
- Output: 30
- Explanation: [5,2,-,10,15] => 5 + 10 + 15 = 30

### Example 2:
- Input: ops = ["5","-2","4","C","D","9","+","+"]
- Output: 27

## Constraints
- 1 <= operations.length <= 1000
- operations[i] is "C", "D", "+", or a string representing an integer.
- The answer is guaranteed to fit in a 32-bit integer.

## Solution
```java
public class Solution {
	public int calPoints(String[] operations) {
		Stack<Integer> stack = new Stack<>();
		for (String op : operations) {
			if (op.equals("+")) {
				int top = stack.pop();
				int newTop = top + stack.peek();
				stack.push(top);
				stack.push(newTop);
			} else if (op.equals("D")) {
				stack.push(2 * stack.peek());
			} else if (op.equals("C")) {
				stack.pop();
			} else {
				stack.push(Integer.parseInt(op));
			}
		}
		int sum = 0;
		for (int score : stack) {
			sum += score;
		}
		return sum;
	}
}
```

Approach

- Use a stack to keep track of valid scores.
- For each operation:
  - If it's an integer, push it onto the stack.
  - If "+", pop the top, add it to the new top, push both back.
  - If "D", push double the top value.
  - If "C", pop the top value.
- Sum all values in the stack at the end.

Time Complexity

O(n), where n is the number of operations.

Space Complexity

O(n), for the stack storing scores.
