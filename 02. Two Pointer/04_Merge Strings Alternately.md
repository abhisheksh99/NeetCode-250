# 1768. Merge Strings Alternately
**Easy**

## Problem Statement
You are given two strings `word1` and `word2`. Merge the strings by adding letters in alternating order, starting with `word1`. If a string is longer than the other, append the additional letters onto the end of the merged string.
Return the merged string.

## Examples
### Example 1:
- Input: word1 = "abc", word2 = "pqr"
- Output: "apbqcr"

### Example 2:
- Input: word1 = "ab", word2 = "pqrs"
- Output: "apbqrs"

### Example 3:
- Input: word1 = "abcd", word2 = "pq"
- Output: "apbqcd"

## Constraints
- 1 <= word1.length, word2.length <= 100
- word1 and word2 consist of lowercase English letters.

## Solution
```java
class Solution {
	public String mergeAlternately(String word1, String word2) {
		int n=word1.length(),m=word2.length();
		StringBuilder res = new StringBuilder();
		for(int i=0;i<n||i<m;i++){
			if(i<n){
				res.append(word1.charAt(i));
			}
			if(i<m){
				res.append(word2.charAt(i));
			}
		}
		return res.toString();
	}
}
```

Approach

- Use a single loop to iterate up to the maximum length of the two strings.
- At each step, if the current index is within the bounds of `word1`, append its character to the result.
- If the current index is within the bounds of `word2`, append its character to the result.
- Continue until all characters from both strings are merged.
- Notes: Handles strings of unequal length by appending the remaining characters from the longer string.

Time Complexity

O(n + m), where n and m are the lengths of the two strings.

Space Complexity

O(n + m), for the result string.
