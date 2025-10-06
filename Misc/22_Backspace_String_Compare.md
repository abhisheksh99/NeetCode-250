# 844. Backspace String Compare

## Problem Statement
Given two strings `s` and `t`, return `true` if they are equal when both are typed into empty text editors. `'#'` means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

### Examples

#### Example 1:
```
Input: s = "ab#c", t = "ad#c"
Output: true
Explanation: Both s and t become "ac".
```

#### Example 2:
```
Input: s = "ab##", t = "c#d#"
Output: true
Explanation: Both s and t become "".
```

#### Example 3:
```
Input: s = "a#c", t = "b"
Output: false
Explanation: s becomes "c" while t becomes "b".
```

## Approach
This problem can be solved using a two-pointer approach to efficiently process the strings from the end to the beginning, handling backspaces without explicitly building the final strings.

1. **Two-Pointer Technique**:
   - Start from the end of both strings.
   - For each string, skip characters that are backspaced.
   - Compare the remaining characters one by one.

2. **Key Insight**:
   - By processing from the end, we can handle backspaces more efficiently because we know exactly which characters to skip.
   - This approach avoids using extra space to build the final strings.

### Solution Code
```java
class Solution {
    public boolean backspaceCompare(String s, String t) {
        int i = s.length() - 1, j = t.length() - 1;
        int skipS = 0, skipT = 0;

        while (i >= 0 || j >= 0) {
            // Find the next valid character in s
            while (i >= 0) {
                if (s.charAt(i) == '#') {
                    skipS++;
                    i--;
                } else if (skipS > 0) {
                    skipS--;
                    i--;
                } else {
                    break;
                }
            }

            // Find the next valid character in t
            while (j >= 0) {
                if (t.charAt(j) == '#') {
                    skipT++;
                    j--;
                } else if (skipT > 0) {
                    skipT--;
                    j--;
                } else {
                    break;
                }
            }

            // Compare the current characters
            if (i >= 0 && j >= 0) {
                if (s.charAt(i) != t.charAt(j)) {
                    return false;
                }
            } else if (i >= 0 || j >= 0) {
                // One string has more characters than the other
                return false;
            }
            
            i--;
            j--;
        }
        
        return true;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n + m), where n and m are the lengths of strings `s` and `t` respectively. We process each character at most twice.
- **Space Complexity**: O(1), as we use a constant amount of extra space.

## Key Insights
1. **Efficient Backspace Handling**: By processing from the end, we can efficiently skip characters that are backspaced without modifying the original strings.
2. **No Extra Space**: The solution does not require additional space to store the final strings, making it memory efficient.
3. **Early Termination**: The comparison stops as soon as a mismatch is found, which can save time for large inputs.

## Edge Cases
1. **Empty Strings**: Both strings are empty.
2. **All Backspaces**: Strings consist only of backspaces.
3. **Multiple Backspaces**: Consecutive backspaces in the strings.
4. **Backspacing Beyond Start**: More backspaces than characters in the string.

## Follow-up
How would you solve this problem if the backspace character could be escaped (i.e., `\#` represents a literal `#`)?