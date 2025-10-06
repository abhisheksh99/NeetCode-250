# Stack: Concepts, Operations, and Templates

## What is a Stack?
A stack is a linear data structure that follows the Last-In-First-Out (LIFO) principle. The last element added (pushed) to the stack is the first one to be removed (popped).

## Stack Operations
- **Push**: Insert an element onto the top of the stack.
- **Pop**: Remove and return the top element from the stack.
- **Peek/Top**: Return the top element without removing it.
- **isEmpty**: Check if the stack is empty.
- **Size**: Return the number of elements in the stack.

## Common Stack Functions in Java
| Operation | Java Method |
|-----------|------------|
| Push      | `stack.push(x)` |
| Pop       | `stack.pop()`   |
| Peek/Top  | `stack.peek()`  |
| isEmpty   | `stack.isEmpty()` |
| Size      | `stack.size()` |


## Java Stack Template (Integer)
```java
import java.util.Stack;

Stack<Integer> stack = new Stack<>();
stack.push(10); // Push
int top = stack.pop(); // Pop
int peek = stack.peek(); // Peek
boolean empty = stack.isEmpty(); // isEmpty
int size = stack.size(); // Size
```

## Java Generic Stack Template
```java
import java.util.Stack;

// Stack for any type T
Stack<T> stack = new Stack<>();

// Example: Stack of Strings
Stack<String> stringStack = new Stack<>();
stringStack.push("hello");
String s = stringStack.pop();

// Example: Stack of custom objects
Stack<MyClass> myClassStack = new Stack<>();
myClassStack.push(new MyClass());
MyClass obj = myClassStack.peek();
```

## Monotonic Stack Templates

### Increasing Monotonic Stack (for next smaller/greater element)
```java
// Monotonic Increasing Stack (stores indices)
Stack<Integer> stack = new Stack<>();
for (int i = 0; i < arr.length; i++) {
	while (!stack.isEmpty() && arr[stack.peek()] >= arr[i]) {
		stack.pop();
	}
	// stack.peek() is the index of the previous smaller element for arr[i]
	stack.push(i);
}
```

### Decreasing Monotonic Stack (for next greater/smaller element)
```java
// Monotonic Decreasing Stack (stores indices)
Stack<Integer> stack = new Stack<>();
for (int i = 0; i < arr.length; i++) {
	while (!stack.isEmpty() && arr[stack.peek()] <= arr[i]) {
		stack.pop();
	}
	// stack.peek() is the index of the previous greater element for arr[i]
	stack.push(i);
}
```

## Time and Space Complexity
- **Push**: O(1)
- **Pop**: O(1)
- **Peek/Top**: O(1)
- **isEmpty/Size**: O(1)
- **Monotonic Stack Construction**: O(n) for n elements (each element is pushed and popped at most once)
- **Space Complexity**: O(n) in the worst case (all elements in the stack)
