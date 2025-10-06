# 424. Longest Repeating Character Replacement
**Medium**

## Problem Statement
You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. Do this at most `k` times.
Return the length of the longest substring containing the same letter you can get after performing the above operations.

## Examples
### Example 1:
- Input: s = "ABAB", k = 2
- Output: 4

### Example 2:
- Input: s = "AABABBA", k = 1
- Output: 4

## Constraints
- 1 <= s.length <= 10^5
- s consists of only uppercase English letters.
- 0 <= k <= s.length

## Solution
```java
class Solution {
	public int characterReplacement(String s, int k) {
		Map<Character, Integer> occurence = new HashMap<>();
		int left = 0, right = 0, ans = 0, maxOccurence = 0;

		while (right < s.length()) {
			char c = s.charAt(right);
			occurence.put(c, occurence.getOrDefault(c, 0) + 1);

			maxOccurence = Math.max(maxOccurence, occurence.get(c));

			if (right - left + 1 - maxOccurence > k) {
				char leftChar = s.charAt(left);
				occurence.put(leftChar, occurence.get(leftChar) - 1);
				left++;
			}

			ans = Math.max(ans, right - left + 1); 
			right++;
		}
		return ans;
	}
}
```

Approach

- Use a sliding window with two pointers, `left` and `right`.
- Track the count of each character in the current window using a HashMap.
- Keep track of the maximum occurrence of any character in the window.
- If the window size minus the max occurrence exceeds `k`, shrink the window from the left.
- Update the answer with the maximum window size found.
- Notes: This approach efficiently finds the longest valid window in a single pass.

Time Complexity

O(n), where n is the length of the string.

Space Complexity

O(1), since the alphabet size is fixed (26 uppercase letters).
