# 125. Valid Palindrome
**Easy**

## Problem Statement
Given a string `s`, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

## Examples
### Example 1:
- Input: s = "A man, a plan, a canal: Panama"
- Output: true

### Example 2:
- Input: s = "race a car"
- Output: false

## Constraints
- 1 <= s.length <= 2 * 10^5
- s consists only of printable ASCII characters.

## Solution
```java
class Solution {
	public boolean isPalindrome(String s) {
		int left=0,right=s.length()-1;

		while(left<right){
			while(left<right && !Character.isLetterOrDigit(s.charAt(left))){
				left++;
			}
			while(left<right && !Character.isLetterOrDigit(s.charAt(right))){
				right--;
			}

			if(Character.toLowerCase(s.charAt(left))!=Character.toLowerCase(s.charAt(right))){
				return false;
			}

			left++;
			right--;

		}
		return true;
        
	}
}
```

Approach

- Use two pointers, `left` at the start and `right` at the end of the string.
- Move `left` forward and `right` backward, skipping non-alphanumeric characters.
- Compare the characters at `left` and `right` (case-insensitive).
- If they differ, return false. If all pairs match, return true.
- Notes: Ignores non-alphanumeric characters and is case-insensitive.

Time Complexity

O(n), where n is the length of the string.

Space Complexity

O(1), as only a few pointers/variables are used.
