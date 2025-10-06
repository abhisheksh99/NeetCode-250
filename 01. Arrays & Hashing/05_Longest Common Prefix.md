# 14. Longest Common Prefix
**Easy**

## Problem Statement
Write a function to find the longest common prefix string amongst an array of strings.
If there is no common prefix, return an empty string "".

## Examples
### Example 1:
- Input: strs = ["flower","flow","flight"]
- Output: "fl"

### Example 2:
- Input: strs = ["dog","racecar","car"]
- Output: ""

## Constraints
- 1 <= strs.length <= 200
- 0 <= strs[i].length <= 200
- strs[i] consists of only lowercase English letters.

## Solution
```java
class Solution {
	public String longestCommonPrefix(String[] strs) {
		if(strs==null||strs.length==0) return "";

		String prefix = strs[0];

		for(int i=1;i<strs.length;i++){
			while(strs[i].indexOf(prefix)!=0){
				prefix=prefix.substring(0,prefix.length()-1);
			}
			if(prefix.isEmpty()) return "";

		}
		return prefix;
        
	}
}
```

Approach

- If the input array is null or empty, return an empty string.
- Initialize the prefix as the first string in the array.
- For each subsequent string:
  - While the current string does not start with the prefix, shorten the prefix by removing its last character.
  - If the prefix becomes empty, return "" (no common prefix).
- After checking all strings, return the prefix.
- Notes: This approach iteratively reduces the prefix until it matches all strings or becomes empty.

Time Complexity

O(S), where S is the sum of all characters in all strings.

Space Complexity

O(1), as only a few variables are used.
