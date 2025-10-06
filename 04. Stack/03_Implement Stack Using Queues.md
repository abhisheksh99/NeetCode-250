# 225. Implement Stack using Queues
**Easy**

## Problem Statement
Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (`push`, `top`, `pop`, and `empty`).

Implement the `MyStack` class:
- `void push(int x)` Pushes element x to the top of the stack.
- `int pop()` Removes the element on the top of the stack and returns it.
- `int top()` Returns the element on the top of the stack.
- `boolean empty()` Returns true if the stack is empty, false otherwise.

You must use only standard queue operations: enqueue to the back, dequeue from the front, size, and is empty.

## Examples
### Example 1:
- Input: ["MyStack","push","push","top","pop","empty"] [[ ],[1],[2],[ ],[ ],[ ]]
- Output: [null,null,null,2,2,false]

## Constraints
- 1 <= x <= 9
- At most 100 calls will be made to each method of MyStack.
- All the calls to pop and top are valid.

## Solution
```java
public class MyStack {
	private Queue<Integer> q;

	public MyStack() {
		q = new LinkedList<>();
	}

	public void push(int x) {
		q.offer(x);
		for (int i = q.size() - 1; i > 0; i--) {
			q.offer(q.poll());
		}
	}

	public int pop() {
		return q.poll();
	}

	public int top() {
		return q.peek();
	}

	public boolean empty() {
		return q.isEmpty();
	}
}
```

Approach

- Use a single queue to simulate stack behavior.
- On `push`, add the new element and rotate the queue so the new element is at the front.
- On `pop`, remove the front element (which is the top of the stack).
- On `top`, peek the front element.
- On `empty`, check if the queue is empty.

Time Complexity

- `push`: O(n), where n is the number of elements in the stack (due to rotation).
- `pop`, `top`, `empty`: O(1).

Space Complexity

O(n), for storing n elements in the queue.
