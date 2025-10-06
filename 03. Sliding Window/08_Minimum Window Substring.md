# 76. Minimum Window Substring
**Hard**

## Problem Statement
Given two strings `s` and `t` of lengths `m` and `n` respectively, return the minimum window substring of `s` such that every character in `t` (including duplicates) is included in the window. If there is no such substring, return the empty string `""`.

The testcases will be generated such that the answer is unique.

## Examples
### Example 1:
- Input: s = "ADOBECODEBANC", t = "ABC"
- Output: "BANC"

### Example 2:
- Input: s = "a", t = "a"
- Output: "a"

### Example 3:
- Input: s = "a", t = "aa"
- Output: ""

## Constraints
- m == s.length
- n == t.length
- 1 <= m, n <= 10^5
- s and t consist of uppercase and lowercase English letters.

## Solution
```java
class Solution {
	public String minWindow(String s, String t) {
		if (s.length() < t.length()) return "";

		Map<Character, Integer> needed = new HashMap<>();
		for (char c : t.toCharArray()) {
			needed.put(c, needed.getOrDefault(c, 0) + 1);
		}

		int required = needed.size();
		Map<Character, Integer> window = new HashMap<>();
		int formed = 0;

		int left = 0, right = 0;
		int minLen = Integer.MAX_VALUE;
		int start = 0;

		while (right < s.length()) {
			char c = s.charAt(right);
			window.put(c, window.getOrDefault(c, 0) + 1);

			if (needed.containsKey(c) && window.get(c).intValue() == needed.get(c).intValue()) {
				formed++;
			}

			while (left <= right && formed == required) {
				if (right - left + 1 < minLen) {
					minLen = right - left + 1;
					start = left;
				}

				char leftChar = s.charAt(left);
				window.put(leftChar, window.get(leftChar) - 1);
				if (needed.containsKey(leftChar) && window.get(leftChar).intValue() < needed.get(leftChar).intValue()) {
					formed--;
				}
				left++;
			}
			right++;
		}

		return minLen == Integer.MAX_VALUE ? "" : s.substring(start, start + minLen);
	}
}
```

Approach

- Use two hash maps: one for the required character counts (`needed`), and one for the current window (`window`).
- Expand the window by moving `right` and updating the window map.
- When all required characters are matched (using a `formed` counter), try to shrink the window from the left to find the minimum.
- Update the minimum window length and starting index when a valid window is found.
- Return the minimum window substring or an empty string if no valid window exists.

Time Complexity

O(m + n), where m is the length of `s` and n is the length of `t`.

Space Complexity

O(m + n), for the hash maps used to store character counts.
