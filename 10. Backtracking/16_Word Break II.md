# Word Break II

**Difficulty:** Hard

## Problem Statement
Given a string s and a dictionary of strings wordDict, add spaces in s to construct a sentence where each word is a valid dictionary word. Return all such possible sentences in any order.

Note that the same word in the dictionary may be reused multiple times in the segmentation.

**Example:**
```
Input: s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]
Output: ["cats and dog","cat sand dog"]
```

**Constraints:**
- 1 <= s.length <= 20
- 1 <= wordDict.length <= 1000
- 1 <= wordDict[i].length <= 10
- s and wordDict[i] consist of only lowercase English letters.

---

## Java Solution
```java
class Solution {
	public List<String> wordBreak(String s, List<String> wordDict) {
		Set<String> wordSet = new HashSet<>(wordDict);
		Map<String, List<String>> memo = new HashMap<>();
		return backtrack(s, wordSet, memo);
	}

	private List<String> backtrack(String s, Set<String> wordSet, Map<String, List<String>> memo) {
		if (memo.containsKey(s)) return memo.get(s);
		List<String> res = new ArrayList<>();
		if (s.isEmpty()) {
			res.add(""); // base case: empty string returns empty sentence
			return res;
		}
		for (int i = 1; i <= s.length(); i++) {
			String prefix = s.substring(0, i);
			if (wordSet.contains(prefix)) {
				String suffix = s.substring(i);
				List<String> suffixWays = backtrack(suffix, wordSet, memo);
				for (String way : suffixWays) {
					if (way.isEmpty()) {
						res.add(prefix);
					} else {
						res.add(prefix + " " + way);
					}
				}
			}
		}
		memo.put(s, res);
		return res;
	}
}
```

---

## Approach
- Use backtracking with memoization to explore all possible ways to break the string into valid words.
- For each prefix of the string, if it is in the dictionary, recursively solve for the suffix.
- Combine the prefix with all valid ways to break the suffix.
- Use a memoization map to avoid recomputation for the same substring.

---

## Time and Space Complexity
- **Time Complexity:** Exponential in the worst case, but memoization helps reduce redundant work.
- **Space Complexity:** O(n^2) for the memoization map and recursion stack, where n is the length of s.
