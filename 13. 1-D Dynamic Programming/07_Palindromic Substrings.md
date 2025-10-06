# 647. Palindromic Substrings

## Problem Statement
Given a string `s`, return the number of palindromic substrings in it.

A string is a palindrome when it reads the same backward as forward.

A substring is a contiguous sequence of characters within the string.

### Examples

**Example 1:**
```
Input: s = "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
```

**Example 2:**
```
Input: s = "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

### Constraints:
- `1 <= s.length <= 1000`
- `s` consists of lowercase English letters.

## Solution
```java
class Solution {
    public int countSubstrings(String s) {
        if (s == null || s.length() == 0) return 0;

        int count = 0;

        // Try every possible center
        for (int i = 0; i < s.length(); i++) {
            // Odd-length palindromes (single center)
            count += expandFromCenter(s, i, i);
            // Even-length palindromes (double center)
            count += expandFromCenter(s, i, i + 1);
        }

        return count;
    }

    // Helper: expand around center and return number of palindromes found
    private int expandFromCenter(String s, int left, int right) {
        int count = 0;
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            count++;   // Found one palindrome
            left--;
            right++;
        }
        return count;
    }
}
```

## Approach
1. **Expand Around Center**:
   - The key insight is that every palindrome has a center. For a string of length n, there are 2n-1 possible centers (each character and each position between two characters).
   - For each center, we expand outwards as long as the characters on both sides match.
   - We count each valid palindrome found during the expansion.

2. **Two Types of Palindromes**:
   - Odd-length palindromes: centered at a single character (e.g., "aba")
   - Even-length palindromes: centered between two characters (e.g., "abba")

## Complexity
- **Time Complexity**: O(n²)
  - For each of the n characters, we might expand up to O(n) times in the worst case (when all characters are the same).
- **Space Complexity**: O(1)
  - We use a constant amount of extra space (a few variables for counting and indices).

## Edge Cases
- Empty string → `0`
- Single character string → `1` (the character itself)
- All characters the same (e.g., "aaaaa") → n*(n+1)/2 palindromic substrings
- No palindromic substrings longer than 1 character (e.g., "abc")

## Key Insights
- By considering each character (and each pair of characters) as potential centers, we can efficiently count all palindromic substrings.
- The solution avoids redundant checks by expanding from each center only once.
- The space optimization makes it efficient for large input sizes (up to 1000 characters).