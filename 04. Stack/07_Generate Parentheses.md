# 22. Generate Parentheses
**Medium**

## Problem Statement
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

## Examples
### Example 1:
- Input: n = 3
- Output: ["((()))","(()())","(())()","()(())","()()()"]

### Example 2:
- Input: n = 1
- Output: ["()"]

## Constraints
- 1 <= n <= 8

## Solution
```java
class Solution {
	public List<String> generateParenthesis(int n) {
		List<String> result = new ArrayList<>();
		generate(result, "", 0, 0, n);
		return result;
	}

	private void generate(List<String> result, String current, int open, int close, int max) {
		// Base case: if the current string is complete
		if (current.length() == max * 2) {
			result.add(current);
			return;
		}

		// If we can still add an opening bracket
		if (open < max) {
			generate(result, current + "(", open + 1, close, max);
		}

		// If we can add a closing bracket (only if there are unmatched opens)
		if (close < open) {
			generate(result, current + ")", open, close + 1, max);
		}
	}
}
```

Approach

- Use backtracking to build all valid combinations.
- At each step, add an opening parenthesis if possible.
- Add a closing parenthesis if it would not exceed the number of opening ones.
- When the current string reaches length 2*n, add it to the result.

Time Complexity

O(2^n * n), since there are Catalan(n) valid combinations and each is of length 2n.

Space Complexity

O(2^n * n), for storing all valid combinations.
