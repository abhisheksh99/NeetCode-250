# 5. Longest Palindromic Substring

## Problem Statement
Given a string `s`, return the longest palindromic substring in `s`. A string is called a palindrome if it reads the same backward as forward.

### Examples

**Example 1:**
```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

**Example 2:**
```
Input: s = "cbbd"
Output: "bb"
```

### Constraints:
- `1 <= s.length <= 1000`
- `s` consists of only digits and English letters.

## Solution
```java
class Solution {
    public String longestPalindrome(String s) {
        // Base case: empty or null string
        if (s == null || s.length() < 1) {
            return "";
        }

        int start = 0;
        int end = 0;

        // Expand around each character as a potential palindrome center
        for (int i = 0; i < s.length(); i++) {
            // Case 1: odd-length palindrome (single center)
            int len1 = expandFromCenter(s, i, i);
            // Case 2: even-length palindrome (two-character center)
            int len2 = expandFromCenter(s, i, i + 1);

            // Get the maximum length between odd and even length palindromes
            int len = Math.max(len1, len2);

            // Update the longest palindrome boundaries if a longer one is found
            if (len > end - start) {
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }

        // Return the longest palindromic substring
        return s.substring(start, end + 1);
    }

    // Helper method: expands from the given center and returns palindrome length
    private int expandFromCenter(String s, int left, int right) {
        // Expand outwards while characters match and we're within bounds
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        // Return the length of the palindrome found
        // Subtract 1 because we go one step too far in both directions
        return right - left - 1;
    }
}
```

## Approach
1. **Expand Around Center**:
   - The key insight is that a palindrome mirrors around its center. Therefore, we can check for palindromes by expanding around each character (for odd-length palindromes) and each pair of characters (for even-length palindromes).
   - For each character in the string, we consider it as the center of a potential palindrome and expand outwards to find the longest palindrome with that center.
   - We need to handle both odd-length and even-length palindromes, which is why we make two calls to `expandFromCenter` for each position.

2. **Helper Method**:
   - The `expandFromCenter` method takes a string and two indices (left and right) and expands outwards as long as the characters at these indices match and we're within the string bounds.
   - It returns the length of the palindrome found.

## Complexity
- **Time Complexity**: O(n²)
  - We have a loop that runs for each character in the string (O(n)), and for each character, we might expand up to O(n) times in the worst case (when the entire string is a palindrome).
- **Space Complexity**: O(1)
  - We use only a constant amount of extra space (a few variables to store indices and lengths).

## Edge Cases
- Empty string → `""`
- Single character string → the character itself
- Entire string is a palindrome → the entire string
- Multiple palindromes of the same maximum length → any one of them is valid
- No palindromes longer than 1 character → any single character

## Key Insights
- By considering each character (and each pair of characters) as the center of a potential palindrome, we can efficiently find the longest palindromic substring.
- The solution efficiently handles both odd and even length palindromes by checking two different center configurations for each position.
- The space-optimized approach avoids using additional data structures, making it memory efficient.