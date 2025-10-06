# 139. Word Break

## Problem Statement
Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

Note that the same word in the dictionary may be reused multiple times in the segmentation.

### Examples

**Example 1:**
```
Input: s = "leetcode", wordDict = ["leet","code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

**Example 2:**
```
Input: s = "applepenapple", wordDict = ["apple","pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.
```

**Example 3:**
```
Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
Output: false
```

### Constraints:
- `1 <= s.length <= 300`
- `1 <= wordDict.length <= 1000`
- `1 <= wordDict[i].length <= 20`
- `s` and `wordDict[i]` consist of only lowercase English letters.
- All the strings of `wordDict` are unique.

## Solution
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        boolean[] dp = new boolean[s.length()+1];
        dp[0] = true;
        Set<String> wordSet = new HashSet<>(wordDict);
        
        for(int i = 1; i <= s.length(); i++) {
            for(int j = i-1; j >= 0; j--) {
                if(dp[j] && wordSet.contains(s.substring(j,i))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        
        return dp[s.length()];
    }
}
```

## Approach
1. **Dynamic Programming**:
   - We use a boolean array `dp` where `dp[i]` represents whether the substring `s[0...i-1]` can be segmented into dictionary words.
   - Initialize `dp[0]` to `true` since an empty string can always be segmented.
   - Convert the word dictionary to a `HashSet` for O(1) lookups.
   - For each position `i` in the string, check all possible substrings ending at `i`:
     - If `dp[j]` is `true` (meaning the substring up to `j` is valid) and the substring `s[j...i-1]` is in the dictionary, then `dp[i]` is `true`.
     - We can break early once we find a valid segmentation for position `i`.
   - The final result is stored in `dp[s.length()]`.

## Complexity
- **Time Complexity**: O(n²)
  - Where n is the length of the string `s`.
  - For each position `i`, we check up to `i` previous positions.
- **Space Complexity**: O(n + m)
  - `O(n)` for the `dp` array.
  - `O(m)` for the `HashSet` containing the dictionary words, where m is the total number of characters in the dictionary.

## Edge Cases
- `s = "a"`, `wordDict = ["a"]` → `true`
- `s = "aaaaaaa"`, `wordDict = ["aaa","aaaa"]` → `true`
- `s = "catsandog"`, `wordDict = ["cats","dog","sand","and","cat"]` → `false`
- `s` contains characters not in any dictionary word → `false`

## Key Insights
- The problem can be broken down into smaller subproblems, making it suitable for dynamic programming.
- By using a `HashSet` for the dictionary, we achieve O(1) lookups, which is more efficient than searching through the list.
- The solution efficiently determines if the string can be segmented by checking all possible prefixes against the dictionary and using previously computed results.
- The early termination in the inner loop (`break` when `dp[i]` becomes `true`) optimizes the solution by avoiding unnecessary checks.