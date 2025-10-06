# 232. Implement Queue using Stacks
**Easy**

## Problem Statement
Implement a first-in-first-out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `pop`, `peek`, and `empty`).

Implement the `MyQueue` class:
- `void push(int x)` Pushes element x to the back of the queue.
- `int pop()` Removes the element from the front of the queue and returns it.
- `int peek()` Returns the element at the front of the queue.
- `boolean empty()` Returns true if the queue is empty, false otherwise.

You must use only standard stack operations: push to top, pop from top, peek, and is empty.

## Examples
### Example 1:
- Input: ["MyQueue","push","push","peek","pop","empty"] [[ ],[1],[2],[ ],[ ],[ ]]
- Output: [null,null,null,1,1,false]

## Constraints
- 1 <= x <= 9
- At most 100 calls will be made to each method of MyQueue.
- All the calls to pop and peek are valid.

## Solution
```java
public class MyQueue {
	private Stack<Integer> stack1;
	private Stack<Integer> stack2;

	public MyQueue() {
		stack1 = new Stack<>();
		stack2 = new Stack<>();
	}

	public void push(int x) {
		stack1.push(x);
	}

	public int pop() {
		while (stack1.size() > 1) {
			stack2.push(stack1.pop());
		}
		int res = stack1.pop();
		while (!stack2.isEmpty()) {
			stack1.push(stack2.pop());
		}
		return res;
	}

	public int peek() {
		while (stack1.size() > 1) {
			stack2.push(stack1.pop());
		}
		int res = stack1.peek();
		while (!stack2.isEmpty()) {
			stack1.push(stack2.pop());
		}
		return res;
	}

	public boolean empty() {
		return stack1.isEmpty();
	}
}
```

Approach

- Use two stacks to simulate queue behavior.
- On `push`, add the element to `stack1`.
- On `pop` or `peek`, move all but the last element from `stack1` to `stack2`, then pop/peek the last element, and move everything back.
- On `empty`, check if `stack1` is empty.

Time Complexity

- `push`: O(1)
- `pop`, `peek`: O(n) in the worst case (moving all elements between stacks)
- `empty`: O(1)

Space Complexity

O(n), for storing n elements in the two stacks.
