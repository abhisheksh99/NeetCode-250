# 10. Regular Expression Matching

## Problem Statement
Given an input string `s` and a pattern `p`, implement regular expression matching with support for `'.'` and `'*'` where:
- `'.'` Matches any single character.
- `'*'` Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

## Examples

### Example 1:
```
Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

### Example 2:
```
Input: s = "aa", p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

### Example 3:
```
Input: s = "ab", p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

## Approach
We use dynamic programming to solve this problem:
1. Create a 2D DP array where `dp[i][j]` represents if the first `i` characters of `s` match the first `j` characters of `p`
2. Initialize `dp[0][0]` as true since empty string matches empty pattern
3. Handle patterns with '*' that can match empty strings (like "a*" can match empty string)
4. For each character in the string and pattern:
   - If current characters match or pattern has '.', carry over the previous state
   - If current pattern is '*':
     - If preceding character doesn't match, consider zero occurrence
     - Otherwise, consider zero, one, or multiple occurrences of the preceding character
5. The result is in `dp[m][n]` where `m` is length of `s` and `n` is length of `p`

## Solution Code
```java
class Solution {
    public boolean isMatch(String s, String p) {
        if (s == null || p == null) {
            return false;
        }
        
        boolean[][] dp = new boolean[s.length() + 1][p.length() + 1];
        dp[0][0] = true;
        
        // Handle patterns like a*, a*b*, a*b*c* that can match empty string
        for (int i = 0; i < p.length(); i++) {
            if (p.charAt(i) == '*' && dp[0][i - 1]) {
                dp[0][i + 1] = true;
            }
        }
        
        for (int i = 0; i < s.length(); i++) {
            for (int j = 0; j < p.length(); j++) {
                // If current characters match or pattern has '.', carry over the previous state
                if (p.charAt(j) == '.') {
                    dp[i + 1][j + 1] = dp[i][j];
                }
                if (p.charAt(j) == s.charAt(i)) {
                    dp[i + 1][j + 1] = dp[i][j];
                }
                
                // Handle '*' in pattern
                if (p.charAt(j) == '*') {
                    // If preceding character doesn't match, consider zero occurrence
                    if (p.charAt(j - 1) != s.charAt(i) && p.charAt(j - 1) != '.') {
                        dp[i + 1][j + 1] = dp[i + 1][j - 1];
                    } else {
                        // Consider zero, one, or multiple occurrences of the preceding character
                        // dp[i+1][j-1]: zero occurrence
                        // dp[i+1][j]: one occurrence
                        // dp[i][j+1]: multiple occurrences
                        dp[i + 1][j + 1] = (dp[i + 1][j] || dp[i][j + 1] || dp[i + 1][j - 1]);
                    }
                }
            }
        }
        
        return dp[s.length()][p.length()];
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(m × n) - We fill a 2D array of size (m+1) × (n+1)
- **Space Complexity**: O(m × n) - For the DP table

## Key Insights
1. **DP State Definition**: `dp[i][j]` represents if `s[0..i-1]` matches `p[0..j-1]`
2. **Base Case**: Empty string matches empty pattern
3. **Special Characters**:
   - `'.'` matches any single character
   - `'*'` matches zero or more of the preceding element
4. **State Transition**:
   - For matching characters or '.', carry over the previous state
   - For '*', consider zero, one, or multiple occurrences of the preceding character

## Edge Cases
1. Empty string and empty pattern (should return true)
2. Empty string with pattern that can match empty string (e.g., "a*")
3. Pattern longer than string
4. Multiple consecutive '*' in pattern
5. String with repeating characters and patterns like ".*"

## Follow-up
How would you modify the solution to also support the '+' operator which matches one or more of the preceding element?