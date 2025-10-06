# 271. Encode and Decode Strings
**Medium**

## Problem Statement
Design an algorithm to encode a list of strings to a single string. The encoded string is then decoded back to the original list of strings.

## Examples
### Example 1:
- Input: ["neet","code","love","you"]
- Output: ["neet","code","love","you"]

### Example 2:
- Input: ["hello","world"]
- Output: ["hello","world"]

## Constraints
- 0 <= strs.length <= 1000
- 0 <= strs[i].length <= 200
- strs[i] contains any possible characters.

## Solution
```java
class Solution {
	public String encode(List<String> strs) {
		StringBuilder sb = new StringBuilder();
		for (String s : strs) {
			sb.append(s.length()).append('#').append(s);
		}
		return sb.toString();
	}

	public List<String> decode(String s) {
		List<String> result = new ArrayList<>();
		int i = 0;
		while (i < s.length()) {
			int j = s.indexOf('#', i);
			int length = Integer.parseInt(s.substring(i, j));
			result.add(s.substring(j + 1, j + 1 + length));
			i = j + 1 + length;
		}
		return result;
	}
}
```

Approach

- To encode, for each string, append its length, a delimiter (e.g., '#'), and the string itself to a StringBuilder.
- Concatenate all encoded strings into a single string.
- To decode, iterate through the encoded string:
  - Find the delimiter to extract the length of the next string.
  - Use the length to extract the original string and add it to the result list.
  - Repeat until the end of the encoded string.
- Notes: This approach handles any characters in the original strings, including the delimiter.

Time Complexity

O(N), where N is the total number of characters in all strings.

Space Complexity

O(N), for the encoded string and the decoded list.
