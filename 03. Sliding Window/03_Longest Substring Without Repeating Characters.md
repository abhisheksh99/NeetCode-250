# 3. Longest Substring Without Repeating Characters
**Medium**

## Problem Statement
Given a string `s`, find the length of the longest substring without repeating characters.

## Examples
### Example 1:
- Input: s = "abcabcbb"
- Output: 3
- Explanation: The answer is "abc", with the length of 3.

### Example 2:
- Input: s = "bbbbb"
- Output: 1
- Explanation: The answer is "b", with the length of 1.

### Example 3:
- Input: s = "pwwkew"
- Output: 3
- Explanation: The answer is "wke", with the length of 3.

## Constraints
- 0 <= s.length <= 5 * 10^4
- s consists of English letters, digits, symbols and spaces.

## Solution
```java
class Solution {
	public int lengthOfLongestSubstring(String s) {
		if(s==null || s.length()==0){
			return 0;
		}
		if(s.length()==1){
			return 1;
		}

		int left=0,right=0,ans=0;
		HashSet<Character> set = new HashSet<>();

		while(right<s.length()){
			char c=s.charAt(right);
			while(set.contains(c)){
				set.remove(s.charAt(left));
				left++;
			}
			set.add(c);
			ans=Math.max(ans,right-left+1);
			right++;
		}
		return ans;
	}
}
```

Approach

- Use a HashSet to track unique characters in the current window.
- Use two pointers, `left` and `right`, to define the window.
- Expand the window by moving `right` and adding characters to the set.
- If a duplicate is found, shrink the window from the left until the duplicate is removed.
- Update the maximum window size at each step.
- Notes: This approach efficiently finds the longest substring without repeats in a single pass.

Time Complexity

O(n), where n is the length of the string.

Space Complexity

O(min(n, m)), where m is the size of the character set (e.g., 26 for lowercase letters, 128 for ASCII).
