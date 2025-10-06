# 567. Permutation in String
**Medium**

## Problem Statement
Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, or `false` otherwise.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

## Examples
### Example 1:
- Input: s1 = "ab", s2 = "eidbaooo"
- Output: true
- Explanation: s2 contains one permutation of s1 ("ba").

### Example 2:
- Input: s1 = "ab", s2 = "eidboaoo"
- Output: false

## Constraints
- 1 <= s1.length, s2.length <= 10^4
- s1 and s2 consist of lowercase English letters.

## Solution
```java
class Solution {
	public boolean checkInclusion(String s1, String s2) {
		if (s1.length() > s2.length()) {
			return false;
		}

		int[] s1Map = new int[26];
		int[] s2Map = new int[26];

		for (int i = 0; i < s1.length(); i++) {
			s1Map[s1.charAt(i) - 'a']++;
			s2Map[s2.charAt(i) - 'a']++;
		}

		for (int i = 0; i < s2.length() - s1.length(); i++) {
			if (matches(s1Map, s2Map)) {
				return true;
			}
			s2Map[s2.charAt(i + s1.length()) - 'a']++;
			s2Map[s2.charAt(i) - 'a']--;
		}

		return matches(s1Map, s2Map);
	}

	private boolean matches(int[] s1Map, int[] s2Map) {
		for (int i = 0; i < 26; i++) {
			if (s1Map[i] != s2Map[i]) {
				return false;
			}
		}
		return true;
	}
}
```

Approach

- Use two frequency arrays to count characters in `s1` and the current window of `s2`.
- Initialize both arrays for the first window of size `s1.length()`.
- Slide the window over `s2`, updating the frequency array for each new character and removing the leftmost character.
- After each slide, check if the two frequency arrays match (indicating a permutation).
- Return true if a match is found; otherwise, return false after the loop.

Time Complexity

O(n), where n is the length of `s2`.

Space Complexity

O(1), since the frequency arrays are of fixed size (26 for lowercase English letters).
