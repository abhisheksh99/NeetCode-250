# 2707. Extra Characters in a String
**Medium**

## Problem Statement
You are given a 0-indexed string s and a dictionary of words dictionary. You want to break s into one or more non-overlapping substrings such that each substring is present in dictionary. There may be some extra characters in s which are not present in any dictionary word.

Return the minimum number of extra characters left over after breaking s as described.

## Examples
### Example 1:
- Input: s = "leetscode", dictionary = ["leet","code","leetcode"]
- Output: 1
- Explanation: We can break s as "leet" + "s" + "code". The extra character is 's'.

### Example 2:
- Input: s = "sayhelloworld", dictionary = ["hello","world"]
- Output: 3
- Explanation: We can break s as "say" + "hello" + "world". The extra characters are "say".

## Constraints
- 1 <= s.length <= 50
- 1 <= dictionary.length <= 50
- 1 <= dictionary[i].length <= 50
- dictionary[i] and s consist of only lowercase English letters
- dictionary contains distinct words

## Solution
```java
class Solution {
    public int minExtraChar(String s, String[] dictionary) {
        Set<String> dictionarySet = new HashSet<>();
        for (String word : dictionary) {
            dictionarySet.add(word);
        }
        int n = s.length();
        int[] dp = new int[n + 1];
        dp[0] = 0;  // base case: empty string has 0 extra chars
        for (int i = 1; i <= n; i++) {
            // Option 1: treat s[i-1] as extra character
            dp[i] = dp[i - 1] + 1;
            // Option 2: try to form a dictionary word ending at i-1 index
            for (int j = 0; j < i; j++) {
                String sub = s.substring(j, i);
                if (dictionarySet.contains(sub)) {
                    dp[i] = Math.min(dp[i], dp[j]);
                }
            }
        }
        return dp[n];
    }
}
```

Approach
- Use dynamic programming to keep track of the minimum extra characters for each prefix of s.
- For each position, either treat the current character as extra or match a dictionary word ending at that position.
- The answer is the minimum extra characters for the whole string.

Time Complexity
- O(n^2), where n is the length of s (due to substring checks).

Space Complexity
- O(n + m), where n is the length of s and m is the total length of all dictionary words.
