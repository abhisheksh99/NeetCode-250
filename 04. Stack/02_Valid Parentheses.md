# 20. Valid Parentheses
**Easy**

## Problem Statement
Given a string `s` containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

A string is valid if:
- Open brackets must be closed by the same type of brackets.
- Open brackets must be closed in the correct order.

## Examples
### Example 1:
- Input: s = "()"
- Output: true

### Example 2:
- Input: s = "()[]{}"
- Output: true

### Example 3:
- Input: s = "(]"
- Output: false

## Constraints
- 1 <= s.length <= 10^4
- s consists of parentheses only '()[]{}'.

## Solution
```java
public class Solution {
	public boolean isValid(String s) {
		Stack<Character> stack = new Stack<>();
        
		for (char c : s.toCharArray()) {
			if (c == '(') stack.push(')');
			else if (c == '{') stack.push('}');
			else if (c == '[') stack.push(']');
			else if (stack.isEmpty() || stack.pop() != c) return false;
		}
        
		return stack.isEmpty();
	}
}    
```

Approach

- Use a stack to keep track of expected closing brackets.
- For each character in the string:
  - If it's an opening bracket, push the corresponding closing bracket onto the stack.
  - If it's a closing bracket, check if it matches the top of the stack. If not, return false.
- At the end, the stack should be empty for a valid string.

Time Complexity

O(n), where n is the length of the string.

Space Complexity

O(n), for the stack in the worst case (all opening brackets).
