# Plus One

**Difficulty:** Easy

## Problem Statement  
Given a non-empty array of decimal digits representing a non-negative integer, increment the integer by one and return the resulting array of digits.

Each element in the array contains a single digit. The digits are ordered from most significant to least significant in left-to-right order. The integer does not contain any leading zero, except the number 0 itself.

---

## Example  
Input:  
digits = [1, 2, 3]


Output:  
[1, 2, 4]

Explanation: The array represents the integer 123. Incrementing by one gives 124.

Input:  
digits = [9, 9, 9]

Output:  
[1, 0, 0, 0]


Explanation: The integer 999 incremented by one is 1000.

---

## Constraints  
- `1 <= digits.length <= 100`  
- `0 <= digits[i] <= 9`  
- The digits array does not contain leading zeros except for the number zero itself.

---

## Approach  
- Iterate from the last digit towards the first:  
  - If the current digit is less than 9, increment it by one and return the array immediately (no carry).  
  - If the digit is 9, set it to 0 and continue to the next digit to propagate the carry.  
- If all digits were 9, create a new array with an additional digit at the front set to 1, and the rest default to 0.

---

## Time and Space Complexity  
- **Time Complexity:** O(n), where n is the length of the digits array (in worst case all 9s).  
- **Space Complexity:** O(n) for the new array in the case all digits are 9, otherwise O(1).

---

## Code

```java
public class Solution {
    public static int[] plusOne(int[] digits) {
        // Start from the last digit and move leftward
        for (int i = digits.length - 1; i >= 0; i--) {
            if (digits[i] < 9) {
                digits[i]++;  // No carry needed, just increment and return
                return digits;
            }
            // If the digit is 9, set it to 0 and continue to the next digit
            digits[i] = 0;
        }
        
        // If all digits were 9, we need a new array with an extra digit
        int[] newDigits = new int[digits.length + 1];
        newDigits[0] = 1;  // The most significant digit is 1, rest are 0 by default
        return newDigits;
    }
}
```