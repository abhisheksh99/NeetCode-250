# 97. Interleaving String

## Problem Statement
Given strings `s1`, `s2`, and `s3`, determine if `s3` is formed by an interleaving of `s1` and `s2`.

An interleaving of two strings `s` and `t` is a configuration where they are divided into non-empty substrings such that:
- `s = s1 + s2 + ... + sn`
- `t = t1 + t2 + ... + tm`
- `|n - m| <= 1`
- The interleaving is `s1 + t1 + s2 + t2 + s3 + t3 + ...` or `t1 + s1 + t2 + s2 + t3 + s3 + ...`

## Examples

### Example 1:
```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
Output: true
Explanation: One way to obtain s3 is:
Split s1 = "aa" + "bc" + "c"
Split s2 = "dbbc" + "a"
Interleave: "aa" + "dbbc" + "bc" + "a" + "c" = "aadbbcbcac"
```

### Example 2:
```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
Output: false
```

## Approach
We use dynamic programming to solve this problem:
1. Check if the length of `s3` equals the sum of lengths of `s1` and `s2`
2. Create a 2D boolean DP array where `dp[i][j]` represents if the first `i` characters of `s1` and first `j` characters of `s2` can form the first `i+j` characters of `s3`
3. Initialize the base cases for empty strings
4. Fill the DP table by checking if the current character in `s3` matches either `s1` or `s2` and the previous state is true

## Solution Code
```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        int m = s1.length();
        int n = s2.length();
        int l = s3.length();
        
        // If length of s3 is not equal to the sum of lengths of s1 and s2, return false
        if (m + n != l) {
            return false;
        }

        // Create a 2D boolean array to store the results
        boolean[][] dp = new boolean[m + 1][n + 1];
        
        // Initialize the DP table
        dp[0][0] = true;
        
        // Fill the first row
        for (int i = 1; i <= m; i++) {
            dp[i][0] = dp[i - 1][0] && s1.charAt(i - 1) == s3.charAt(i - 1);
        }
        
        // Fill the first column
        for (int j = 1; j <= n; j++) {
            dp[0][j] = dp[0][j - 1] && s2.charAt(j - 1) == s3.charAt(j - 1);
        }
        
        // Fill the rest of the DP table
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                dp[i][j] = (dp[i - 1][j] && s1.charAt(i - 1) == s3.charAt(i + j - 1)) || 
                          (dp[i][j - 1] && s2.charAt(j - 1) == s3.charAt(i + j - 1));
            }
        }
        
        return dp[m][n];
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(m × n) - Where m is the length of `s1` and n is the length of `s2`
- **Space Complexity**: O(m × n) - For the DP table (can be optimized to O(n) with 1D array)

## Space Optimization
We can optimize the space to O(n) by using a 1D array:
```java
public boolean isInterleave(String s1, String s2, String s3) {
    int m = s1.length(), n = s2.length();
    if (m + n != s3.length()) return false;
    
    boolean[] dp = new boolean[n + 1];
    dp[0] = true;
    
    // Initialize first row
    for (int j = 1; j <= n; j++) {
        dp[j] = dp[j - 1] && s2.charAt(j - 1) == s3.charAt(j - 1);
    }
    
    for (int i = 1; i <= m; i++) {
        dp[0] = dp[0] && s1.charAt(i - 1) == s3.charAt(i - 1);
        for (int j = 1; j <= n; j++) {
            dp[j] = (dp[j] && s1.charAt(i - 1) == s3.charAt(i + j - 1)) || 
                   (dp[j - 1] && s2.charAt(j - 1) == s3.charAt(i + j - 1));
        }
    }
    
    return dp[n];
}
```

## Key Insights
1. **DP State Definition**: `dp[i][j]` represents if first `i` chars of `s1` and first `j` chars of `s2` can form first `i+j` chars of `s3`
2. **State Transition**: The current state depends on either matching with `s1` or `s2` and the previous state
3. **Base Cases**: Empty strings can form an empty string
4. **Character Matching**: At each step, we check if the current character in `s3` matches either `s1` or `s2`

## Edge Cases
1. All three strings are empty
2. `s1` is empty (check if `s2` equals `s3`)
3. `s2` is empty (check if `s1` equals `s3`)
4. `s3` is empty (should return true only if both `s1` and `s2` are empty)
5. Large input sizes (tests efficiency)

## Follow-up
How would you modify the solution to return all possible interleaving combinations of `s1` and `s2` that form `s3`?