# Letter Combinations of a Phone Number

**Difficulty:** Medium

## Problem Statement
Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

**Example:**
```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**Constraints:**
- 0 <= digits.length <= 4
- digits[i] is a digit in the range ['2', '9'].

---

## Java Solution
```java
public class Solution {

	private List<String> res = new ArrayList<>();
	private String[] digitToChar = {
		"", "", "abc", "def", "ghi", "jkl", "mno", "qprs", "tuv", "wxyz"
	};

	public List<String> letterCombinations(String digits) {
		if (digits.isEmpty()) return res;
		backtrack(0, "", digits);
		return res;
	}

	private void backtrack(int i, String curStr, String digits) {
		if (curStr.length() == digits.length()) {
			res.add(curStr);
			return;
		}
		String chars = digitToChar[digits.charAt(i) - '0'];
		for (char c : chars.toCharArray()) {
			backtrack(i + 1, curStr + c, digits);
		}
	}
}
```

---

## Approach
- Use backtracking to generate all possible letter combinations for the given digits.
- For each digit, try all possible corresponding letters and recurse for the next digit.
- When the current combination reaches the length of the input, add it to the result.

---

## Time and Space Complexity
- **Time Complexity:** O(4^n), where n is the length of digits (since each digit can map to up to 4 letters).
- **Space Complexity:** O(4^n) for storing all combinations and O(n) for the recursion stack.
