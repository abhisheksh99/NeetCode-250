# 91. Decode Ways

## Problem Statement
A message containing letters from `A-Z` can be encoded into numbers using the following mapping:
```
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```
Given a string `s` containing only digits, return the number of ways to decode it.

### Examples

**Example 1:**
```
Input: s = "12"
Output: 2
Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).
```

**Example 2:**
```
Input: s = "226"
Output: 3
Explanation: "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

**Example 3:**
```
Input: s = "06"
Output: 0
Explanation: "06" cannot be mapped to 'F' because of the leading zero ("6" is different from "06").
```

### Constraints:
- `1 <= s.length <= 100`
- `s` contains only digits and may contain leading zero(s).

## Solution
```java
class Solution {
    public int numDecodings(String s) {
        // If the first character is '0', there's no valid decoding
        if (s.charAt(0) == '0'){
            return 0;
        }
        
        // one represents the number of ways to decode the previous character
        // two represents the number of ways to decode two characters back
        int one = 1;  // empty string has one way to decode (base case)
        int two = 1;   // single character has one way to decode if not '0'
        
        for(int i = 1; i < s.length(); i++){
            int current = 0;
            
            // If current digit is not '0', we can form a valid single-digit code
            if(s.charAt(i) != '0'){
                current = one;
            }
            
            // Check if current and previous digit form a valid two-digit code (10-26)
            int value = Integer.parseInt(s.substring(i-1, i+1));
            if(value >= 10 && value <= 26){
                current += two;  // Add ways from two characters back
            }
            
            // Update the previous two states
            two = one;
            one = current;
        }
       
        return one;
    }
}
```

## Approach
1. **Dynamic Programming with Space Optimization**:
   - We use two variables `one` and `two` to keep track of the number of ways to decode the string up to the previous character and two characters back, respectively.
   - For each character in the string (starting from the second character), we calculate the number of ways to decode the string up to that point.
   - If the current digit is not '0', it can be decoded as a single digit, so we carry forward the number of ways from the previous character.
   - If the current and previous digit form a valid two-digit number (between 10 and 26), we add the number of ways from two characters back.
   - We update our variables for the next iteration.

## Complexity
- **Time Complexity**: O(n)
  - We iterate through the string once, and each iteration involves constant time operations.
- **Space Complexity**: O(1)
  - We use only a constant amount of extra space (two variables) regardless of the input size.

## Edge Cases
- `s = "0"` → `0` (no valid decoding)
- `s = "10"` → `1` (only "J" is valid)
- `s = "27"` → `1` (only "BG" is valid, not "B" + "7" as '7' is not a valid single-digit code)
- `s = "226"` → `3` ("BZ", "VF", "BBF")
- `s = "12"` → `2` ("AB" or "L")

## Key Insights
- The problem can be broken down into smaller subproblems, making it suitable for dynamic programming.
- We only need to keep track of the last two states, allowing us to optimize space to O(1).
- Leading zeros make a number invalid for decoding (e.g., "06" is invalid).
- A single '0' cannot be decoded, but '10' and '20' are valid as part of two-digit codes.