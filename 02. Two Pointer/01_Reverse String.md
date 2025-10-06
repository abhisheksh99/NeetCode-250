# 344. Reverse String
**Easy**

## Problem Statement
Write a function that reverses a string. The input string is given as an array of characters `s`.
You must do this by modifying the input array in-place with O(1) extra memory.

## Examples
### Example 1:
- Input: s = ["h","e","l","l","o"]
- Output: ["o","l","l","e","h"]

### Example 2:
- Input: s = ["H","a","n","n","a","h"]
- Output: ["h","a","n","n","a","H"]

## Constraints
- 1 <= s.length <= 10^5
- s[i] is a printable ascii character.

## Solution
```java
public class Solution {
	public void reverseString(char[] s) {
		int left = 0, right = s.length - 1;
		while (left < right) {
			char temp = s[left];
			s[left] = s[right];
			s[right] = temp;
			left++;
			right--;
		}
	}
}
```

Approach

- Use two pointers, one at the start (`left`) and one at the end (`right`) of the array.
- Swap the characters at `left` and `right`.
- Move `left` forward and `right` backward, repeating until they meet or cross.
- This reverses the array in-place with O(1) extra space.

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(1), as the reversal is done in-place.
