# 242. Valid Anagram
**Easy**

## Problem Statement
Given two strings s and t, return true if t is an anagram of s, and false otherwise.

## Examples
### Example 1:
- Input: s = "anagram", t = "nagaram"
- Output: true

### Example 2:
- Input: s = "rat", t = "car"
- Output: false

## Constraints
- 1 <= s.length, t.length <= 5 * 10^4
- s and t consist of lowercase English letters.

## Solution
```java
class Solution {
	public boolean isAnagram(String s, String t) {
		if(s.length()!=t.length()) return false;

		int[] charCount = new int[26];

		for(int i=0;i<s.length();i++){
			charCount[s.charAt(i)-'a']++;
			charCount[t.charAt(i)-'a']--;
		}

		for(int i=0;i<26;i++){
			if(charCount[i]!=0){
				return false;
			}
		}
		return true;

	}
}
```

Approach

- If the lengths of s and t are not equal, return false immediately.
- Initialize an integer array `charCount` of size 26 to count character frequencies.
- For each character in s, increment its count in `charCount`.
- For each character in t, decrement its count in `charCount`.
- After processing both strings, check if all values in `charCount` are zero:
  - If yes, s and t are anagrams; return true.
  - If not, return false.
- Notes: Only works for lowercase English letters; single pass over both strings.

Time Complexity

O(n), where n is the length of the strings.

Space Complexity

O(1), since the array size is fixed (26).
