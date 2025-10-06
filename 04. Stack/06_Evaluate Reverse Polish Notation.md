# 150. Evaluate Reverse Polish Notation
**Medium**

## Problem Statement
Evaluate the value of an arithmetic expression in Reverse Polish Notation (RPN).

Valid operators are '+', '-', '*', and '/'. Each operand may be an integer or another expression. Note that division between two integers should truncate toward zero.

## Examples
### Example 1:
- Input: tokens = ["2","1","+","3","*"]
- Output: 9
- Explanation: ((2 + 1) * 3) = 9

### Example 2:
- Input: tokens = ["4","13","5","/","+"]
- Output: 6
- Explanation: (4 + (13 / 5)) = 6

### Example 3:
- Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
- Output: 22

## Constraints
- 1 <= tokens.length <= 10^4
- tokens[i] is either an operator: "+", "-", "*", or "/", or an integer in the range [-200, 200].

## Solution
```java
class Solution {
	public int evalRPN(String[] tokens) {
		Stack<Integer> stack = new Stack<>();
		for(String c:tokens){
			if(c.equals("+")){
				stack.push(stack.pop()+stack.pop());
			}else if(c.equals("-")){
				int a=stack.pop();
				int b=stack.pop();
				stack.push(b-a);
			}else if(c.equals("*")){
				stack.push(stack.pop()*stack.pop());
			}else if(c.equals("/")){
				int a=stack.pop();
				int b=stack.pop();
				stack.push(b/a);
			}else{
				stack.push(Integer.parseInt(c));
			}
		}
		return stack.pop();
	}
}
```

Approach

- Use a stack to evaluate the RPN expression.
- For each token:
  - If it's a number, push it onto the stack.
  - If it's an operator, pop the top two numbers, apply the operator, and push the result back.
- At the end, the stack contains the result.

Time Complexity

O(n), where n is the number of tokens.

Space Complexity

O(n), for the stack storing operands.
