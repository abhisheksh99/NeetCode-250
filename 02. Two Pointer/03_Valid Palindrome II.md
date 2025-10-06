# 680. Valid Palindrome II
**Easy**

## Problem Statement
Given a string `s`, return true if the string can be made into a palindrome by deleting at most one character from it.

## Examples
### Example 1:
- Input: s = "aba"
- Output: true

### Example 2:
- Input: s = "abca"
- Output: true
- Explanation: You could delete the character 'c'.

### Example 3:
- Input: s = "abc"
- Output: false

## Constraints
- 1 <= s.length <= 10^5
- s consists of lowercase English letters.

## Solution
```java
public class Solution {
	public boolean validPalindrome(String s) {
		int l = 0, r = s.length() - 1;

		while (l < r) {
			if (s.charAt(l) != s.charAt(r)) {
				return isPalindrome(s, l + 1, r) || 
					   isPalindrome(s, l, r - 1);
			}
			l++;
			r--;
		}

		return true;
	}

	private boolean isPalindrome(String s, int l, int r) {
		while (l < r) {
			if (s.charAt(l) != s.charAt(r)) {
				return false;
			}
			l++;
			r--;
		}
		return true;
	}
}
```

Approach

- Use two pointers, `l` and `r`, at the start and end of the string.
- Move both pointers towards the center, comparing characters.
- If a mismatch is found, try skipping either the left or right character and check if the remaining substring is a palindrome.
- If either case returns true, the string can be a palindrome by removing at most one character.
- If no mismatches or only one removable character, return true.

Time Complexity

O(n), where n is the length of the string.

Space Complexity

O(1), as only a few pointers/variables are used.
