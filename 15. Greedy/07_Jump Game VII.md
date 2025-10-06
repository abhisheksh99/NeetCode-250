# 1871. Jump Game VII
**Medium**

## Problem Statement
You are given a 0-indexed binary string `s` and two integers `minJump` and `maxJump`. In the beginning, you are standing at index `0`, which is equal to `'0'`. You can move from index `i` to index `j` if the following conditions are fulfilled:

- `i + minJump <= j <= min(i + maxJump, s.length - 1)`, and
- `s[j] == '0'`

Return `true` if you can reach index `s.length - 1` in `s`, or `false` otherwise.

## Examples
### Example 1:
- Input: `s = "011010"`, `minJump = 2`, `maxJump = 3`
- Output: `true`
- Explanation: 
  - Step 1: Move from index 0 to index 3.
  - Step 2: Move from index 3 to index 5.

### Example 2:
- Input: `s = "01101110"`, `minJump = 2`, `maxJump = 3`
- Output: `false`

## Constraints
- `2 <= s.length <= 10^5`
- `s[i]` is either `'0'` or `'1'`
- `s[0] == '0'`
- `1 <= minJump <= maxJump < s.length`

## Solution
```java
class Solution {
    public boolean canReach(String s, int minJump, int maxJump) {
        int n = s.length();
        boolean[] dp = new boolean[n];
        dp[0] = true;
        int count = 0; // number of reachable positions in current window

        for (int i = minJump; i < n; i++) {
            // Add dp value for left bound of current window
            count += dp[i - minJump] ? 1 : 0;

            // Remove dp value for elements that are out of the window's range
            if (i - maxJump - 1 >= 0) {
                count -= dp[i - maxJump - 1] ? 1 : 0;
            }

            // Position i is reachable if there is any reachable position in window and s[i] == '0'
            dp[i] = count > 0 && s.charAt(i) == '0';
        }

        return dp[n - 1];
    }
}
```

## Approach
1. **Sliding Window with Dynamic Programming**:
   - Use a boolean array `dp` where `dp[i]` indicates if position `i` is reachable.
   - Use a sliding window approach to efficiently track the number of reachable positions within the `[i-maxJump, i-minJump]` range.
   - Maintain a `count` variable that tracks the number of reachable positions in the current window.

2. **Key Insights**:
   - A position `i` is reachable if there's at least one reachable position in `[i-maxJump, i-minJump]` and `s[i] == '0'`.
   - The sliding window optimization reduces the time complexity from O(n * (maxJump-minJump)) to O(n).
   - The window slides as we iterate, adding new reachable positions and removing old ones that are out of range.

## Time Complexity
- **O(n)**: We make a single pass through the string, and each operation inside the loop is O(1).

## Space Complexity
- **O(n)**: We use a boolean array of size n to store reachability information.

## Edge Cases
- String of all '0's with minJump = maxJump = 1
- String where the last character is '1'
- Maximum input size (10^5 characters)
- minJump equals maxJump
- minJump = 1, maxJump = n-1 (can jump anywhere)
- Very large difference between minJump and maxJump