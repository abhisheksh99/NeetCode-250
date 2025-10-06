# 72. Edit Distance

## Problem Statement
Given two strings `word1` and `word2`, return the minimum number of operations required to convert `word1` to `word2`.

You have the following three operations permitted on a word:
- Insert a character
- Delete a character
- Replace a character

## Examples

### Example 1:
```
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```

### Example 2:
```
Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```

## Approach
We use dynamic programming to solve this problem:
1. Create a 2D DP array where `dp[i][j]` represents the minimum number of operations to convert `word1[0...i-1]` to `word2[0...j-1]`
2. Initialize the first row and column to represent the number of operations needed to convert to/from an empty string
3. For each character comparison:
   - If characters match: `dp[i][j] = dp[i-1][j-1]` (no operation needed)
   - If characters don't match: take the minimum of three possible operations:
     - Insert: `dp[i][j-1] + 1`
     - Delete: `dp[i-1][j] + 1`
     - Replace: `dp[i-1][j-1] + 1`
4. The answer will be in `dp[m][n]` where `m` is the length of `word1` and `n` is the length of `word2`

## Solution Code
```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();

        // Create a 2D dp array
        int[][] dp = new int[m + 1][n + 1];

        // Initialize dp[i][0] = i (delete all characters) and dp[0][j] = j (insert all characters)
        for (int i = 0; i <= m; i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j <= n; j++) {
            dp[0][j] = j;
        }

        // Fill the dp array
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    // Characters match, no operation needed
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    // Take minimum of insert, delete, or replace operations
                    dp[i][j] = 1 + Math.min(
                        Math.min(dp[i - 1][j],    // Delete
                                dp[i][j - 1]),   // Insert
                        dp[i - 1][j - 1]         // Replace
                    );
                }
            }
        }

        // Return the result
        return dp[m][n];
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(m × n) - We fill a 2D array of size (m+1) × (n+1)
- **Space Complexity**: O(m × n) - For the DP table

## Key Insights
1. **DP State Definition**: `dp[i][j]` represents the minimum edit distance between `word1[0..i-1]` and `word2[0..j-1]`
2. **Base Cases**: 
   - Convert any string to empty: delete all characters
   - Convert empty to any string: insert all characters
3. **State Transition**: 
   - Match: No cost, take diagonal value
   - Mismatch: 1 + min(insert, delete, replace)
4. **Result Extraction**: The answer is in the bottom-right cell of the DP table

## Edge Cases
1. One or both strings are empty
2. Identical strings (should return 0)
3. Completely different strings (all operations needed)
4. One string is a subsequence of the other
5. Large strings (tests efficiency)

## Follow-up
How would you modify the solution to also return the sequence of operations performed to achieve the minimum edit distance?