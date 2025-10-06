# 394. Decode String
**Medium**

## Problem Statement
Given an encoded string, return its decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is repeated exactly k times. k is guaranteed to be a positive integer.

You may assume that the input string is always valid, no extra white spaces, square brackets are well-formed, etc.

## Examples
### Example 1:
- Input: s = "3[a]2[bc]"
- Output: "aaabcbc"

### Example 2:
- Input: s = "3[a2[c]]"
- Output: "accaccacc"

### Example 3:
- Input: s = "2[abc]3[cd]ef"
- Output: "abcabccdcdcdef"

## Constraints
- 1 <= s.length <= 30
- s consists of digits, English letters, and square brackets '[]'.
- s is guaranteed to be a valid input.

## Solution
```java
class Solution {
	public String decodeString(String s) {
		Stack<Integer> countStack = new Stack<>();
		Stack<String> stringStack = new Stack<>();
		StringBuilder current = new StringBuilder();
		int k = 0;

		for (char c : s.toCharArray()) {
			if (Character.isDigit(c)) {
				k = k * 10 + (c - '0'); // handle multi-digit numbers
			} else if (c == '[') {
				countStack.push(k);
				stringStack.push(current.toString());
				current = new StringBuilder();
				k = 0;
			} else if (c == ']') {
				int count = countStack.pop();
				StringBuilder temp = new StringBuilder(stringStack.pop());
				for (int i = 0; i < count; i++) {
					temp.append(current);
				}
				current = temp;
			} else {
				current.append(c);
			}
		}

		return current.toString();
	}
}
```

Approach

- Use two stacks: one for counts and one for strings.
- Iterate through the string:
  - If a digit, build the repeat count.
  - If '[', push the count and current string onto their stacks, reset current string and count.
  - If ']', pop count and previous string, repeat current string count times and append to previous.
  - If a letter, append to current string.
- Return the final built string.

Time Complexity

O(n), where n is the length of the string.

Space Complexity

O(n), for the stacks and the output string.
