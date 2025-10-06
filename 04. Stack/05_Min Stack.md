# 155. Min Stack
**Medium**

## Problem Statement
Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:
- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element val onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

## Examples
### Example 1:
- Input: ["MinStack","push","push","push","getMin","pop","top","getMin"] [[],[-2],[0],[-3],[],[],[],[]]
- Output: [null,null,null,null,-3,null,0,-2]

## Constraints
- -2^31 <= val <= 2^31 - 1
- Methods `pop`, `top`, and `getMin` operations are always called on non-empty stacks.

## Solution
```java
class MinStack {

	private Stack<Integer> stack;

	public MinStack() {
		stack = new Stack<>();
	}

	public void push(int val) {
		stack.push(val);
	}

	public void pop() {
		stack.pop();
	}

	public int top() {
		return stack.peek();
	}

	public int getMin() {
		Stack<Integer> tmp = new Stack<>();
		int mini = stack.peek();
		while (!stack.isEmpty()) {
			mini = Math.min(mini, stack.peek());
			tmp.push(stack.pop());
		}
		while (!tmp.isEmpty()) {
			stack.push(tmp.pop());
		}
		return mini;
	}
}
```

Approach

- Use a single stack to store all values.
- For `getMin()`, iterate through the stack to find the minimum (not optimal, O(n)).
- For better performance, use an auxiliary stack to keep track of the minimum at each level (not shown here).

Time Complexity

- `push`, `pop`, `top`: O(1)
- `getMin`: O(n) (since it scans the stack)

Space Complexity

O(n), for storing n elements in the stack.
