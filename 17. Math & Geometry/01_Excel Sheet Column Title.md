# Excel Sheet Column Title

**Difficulty:** Easy

## Problem Statement  
Given an integer `columnNumber`, return its corresponding column title as it appears in an Excel sheet.

For example:  
A -> 1  
B -> 2  
C -> 3  
...  
Z -> 26  
AA -> 27  
AB -> 28  
...

**Example:**  
Input: columnNumber = 28  
Output: "AB"

**Constraints:**  
- 1 <= columnNumber <= 2^31 - 1

---

## Approach  
- Treat the problem like converting a number to base-26, with a twist: instead of 0-25, Excel columns use 1-26 (hence the `columnNumber--` before modulus).  
- Repeatedly reduce the `columnNumber`:  
  - Subtract 1 to adjust for 1-based indexing.  
  - Take the remainder modulo 26 to find the current letter.  
  - Append the corresponding character to the result.  
  - Divide the number by 26 to move to the next digit.  
- Finally, reverse the result since we computed it backwards.

---

## Time and Space Complexity  
- **Time Complexity:** O(log₍₂₆₎n), where `n` is the input column number (since we're dividing by 26 each step).  
- **Space Complexity:** O(log₍₂₆₎n) for the result string.  

```java
public class Solution {
    public String convertToTitle(int columnNumber) {
        StringBuilder res = new StringBuilder();
        while (columnNumber > 0) {
            columnNumber--; // Adjust from 1-indexed to 0-indexed
            int offset = columnNumber % 26;
            res.append((char) ('A' + offset));
            columnNumber /= 26;
        }
        return res.reverse().toString();
    }
}
```