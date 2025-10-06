# Palindrome Partitioning

**Difficulty:** Medium

## Problem Statement
Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.

**Example:**
```
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
```

**Constraints:**
- 1 <= s.length <= 16
- s consists of only lowercase English letters.

---

## Java Solution
```java
class Solution {
	public List<List<String>> partition(String s) {
		List<List<String>> result = new ArrayList<>();
		backtrack(result, new ArrayList<>(), s, 0);
		return result;
	}

	private void backtrack(List<List<String>> result, List<String> current, String s, int start) {
		if (start == s.length()) {
			result.add(new ArrayList<>(current));
			return;
		}
		for (int end = start; end < s.length(); end++) {
			if (isPalindrome(s, start, end)) {
				current.add(s.substring(start, end + 1));
				backtrack(result, current, s, end + 1);
				current.remove(current.size() - 1);
			}
		}
	}

	private boolean isPalindrome(String s, int start, int end) {
		while (start < end) {
			if (s.charAt(start++) != s.charAt(end--)) {
				return false;
			}
		}
		return true;
	}
}
```

---

## Approach
- Use backtracking to explore all possible partitions of the string.
- At each step, check if the current substring is a palindrome.
- If it is, add it to the current partition and recurse for the rest of the string.
- When the end of the string is reached, add the current partition to the result.

---

## Time and Space Complexity
- **Time Complexity:** O(n * 2^n), where n is the length of s (since there are 2^n possible partitions and each substring check is O(n)).
- **Space Complexity:** O(n * 2^n) for storing all partitions and O(n) for the recursion stack.
