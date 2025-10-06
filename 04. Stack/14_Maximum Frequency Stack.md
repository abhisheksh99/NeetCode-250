# 895. Maximum Frequency Stack
**Hard**

## Problem Statement
Design a stack-like data structure to push elements to the stack and pop the most frequent element from the stack.

Implement the `FreqStack` class:
- `FreqStack()` constructs an empty frequency stack.
- `void push(int val)` pushes an integer val onto the stack.
- `int pop()` removes and returns the most frequent element in the stack. If there is a tie for the most frequent element, the element closest to the top of the stack is removed and returned.

## Examples
### Example 1:
- Input: ["FreqStack","push","push","push","push","push","pop","pop","pop","pop"] [[],[5],[7],[5],[7],[4],[5],[7],[5],[4]]
- Output: [null,null,null,null,null,null,5,7,5,4]

## Constraints
- 0 <= val <= 10^9
- At most 2 * 10^4 calls will be made to `push` and `pop`.

## Solution
```java
class FreqStack {
	private Map<Integer, Integer> freqMap;
	private Map<Integer, Stack<Integer>> group;
	private int maxFreq;

	public FreqStack() {
		freqMap = new HashMap<>();
		group = new HashMap<>();
		maxFreq = 0;
	}

	public void push(int val) {
		int freq = freqMap.getOrDefault(val, 0) + 1;
		freqMap.put(val, freq);
		if (!group.containsKey(freq)) {
			group.put(freq, new Stack<>());
		}
		group.get(freq).push(val);
		maxFreq = Math.max(maxFreq, freq);
	}

	public int pop() {
		Stack<Integer> stack = group.get(maxFreq);
		int val = stack.pop();
		freqMap.put(val, freqMap.get(val) - 1);
		if (stack.isEmpty()) {
			group.remove(maxFreq);
			maxFreq--;
		}
		return val;
	}
}
```

Approach

- Use a frequency map to track the count of each value.
- Use a group map to track stacks of values for each frequency.
- Track the current maximum frequency.
- On `push`, increment the value's frequency, push it onto the corresponding frequency stack, and update max frequency.
- On `pop`, pop from the stack of the current max frequency, decrement the value's frequency, and update max frequency if needed.

Time Complexity

O(1) for both `push` and `pop` operations.

Space Complexity

O(n), where n is the number of elements in the stack.
