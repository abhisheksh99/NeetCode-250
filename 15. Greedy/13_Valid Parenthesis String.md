# 678. Valid Parenthesis String

## Problem Statement
Given a string `s` containing only three types of characters: '(', ')' and '*', return `true` if `s` is valid.

The following rules define a valid string:
- Any left parenthesis `'('` must have a corresponding right parenthesis `')'`.
- Any right parenthesis `')'` must have a corresponding left parenthesis `'('`.
- `'*'` could be treated as a single right parenthesis `')'`, a single left parenthesis `'('`, or an empty string `""`.
- An empty string is also valid.

### Examples

**Example 1:**
```
Input: s = "()"
Output: true
```

**Example 2:**
```
Input: s = "(*)"
Output: true
```

**Example 3:**
```
Input: s = "(*))"
Output: true
```

### Constraints:
- `1 <= s.length <= 100`
- `s[i]` is `'('`, `')'` or `'*'`.

## Solution
```java
class Solution {
    public boolean checkValidString(String s) {
        int minOpen = 0;  // Minimum possible open parentheses
        int maxOpen = 0;  // Maximum possible open parentheses
        
        for (char c : s.toCharArray()) {
            if (c == '(') {
                // Treat '(' as an open parenthesis
                minOpen++;
                maxOpen++;
            } else if (c == ')') {
                // Treat ')' as a closing parenthesis
                minOpen--;
                maxOpen--;
            } else {
                // '*' can be treated as '(', ')' or ""
                minOpen--;  // Treat '*' as ')'
                maxOpen++;  // Treat '*' as '('
            }
            
            // If at any point, maxOpen becomes negative, it means there are too many ')'
            if (maxOpen < 0) {
                return false;
            }
            
            // minOpen should never be negative, as we cannot have unmatched ')' without '('
            minOpen = Math.max(minOpen, 0);
        }
        
        // If minOpen is 0, it means all '(' can be matched with ')'
        return minOpen == 0;
    }
}
```

## Approach
1. **Track Possible Open Parentheses Range**: We maintain two counters, `minOpen` and `maxOpen`, which represent the minimum and maximum possible number of open parentheses at each step.
2. **Process Each Character**:
   - For `'('`: Both `minOpen` and `maxOpen` are incremented.
   - For `')'`: Both `minOpen` and `maxOpen` are decremented.
   - For `'*'`: We consider it as `'('` (increasing `maxOpen`) or `')'` (decreasing `minOpen`) or an empty string (no change).
3. **Validation Checks**:
   - If `maxOpen` becomes negative at any point, it means there are more closing parentheses than opening ones, making the string invalid.
   - `minOpen` should never be negative, as we can't have unmatched closing parentheses without corresponding opening ones.
4. **Final Check**: If `minOpen` is 0 at the end, it means all opening parentheses can be matched with closing ones.

## Complexity
- **Time Complexity**: O(n), where n is the length of the string. We make a single pass through the string.
- **Space Complexity**: O(1), as we only use a constant amount of extra space.

## Edge Cases
- Empty string should return `true`.
- Strings with only `'*'` characters are always valid.
- Strings with an equal number of `'('` and `')'` are valid regardless of `'*'` positions.
- Strings with more `')'` than `'('` and `'*'` combined are invalid.
- Long strings (up to 100 characters) should be handled efficiently.
- Strings with `'*'` at the beginning or end should be handled correctly.