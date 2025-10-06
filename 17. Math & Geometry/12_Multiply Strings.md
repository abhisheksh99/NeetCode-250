# Multiply Strings

**Difficulty:** Medium

## Problem Statement  
Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also as a string.

You must not use any built-in BigInteger library or convert the inputs to integer directly because the numbers can be very large.

---

## Example  
Input:  
num1 = "123"
num2 = "456"


Output:  
"56088"


---

## Constraints  
- `1 <= num1.length, num2.length <= 200`  
- `num1` and `num2` consist of digits only.  
- Both `num1` and `num2` do not contain any leading zero, except the number 0 itself.

---

## Approach  
- If either `num1` or `num2` is `"0"`, return `"0"` immediately.  
- Use an integer array `result` of size `num1.length() + num2.length()` to store the multiplication results digit by digit.  
- Reverse iterate over both strings to multiply each digit and add the product at the correct position in `result`.  
- Handle carry for each multiplication step.  
- Convert the result array to a string, skipping any leading zeros.

---

## Time and Space Complexity  
- **Time Complexity:** O(m * n), where `m` and `n` are the lengths of `num1` and `num2`.  
- **Space Complexity:** O(m + n), for the array storing the intermediate multiplication results.

---

## Code

```java
public class Solution {
    public String multiply(String num1, String num2) {
        if (num1.equals("0") || num2.equals("0")) return "0";  // If one of the numbers is 0, the result is 0.
        
        // Initialize an array to hold the result of the multiplication.
        int[] result = new int[num1.length() + num2.length()];
        
        // Reverse both num1 and num2 for easier calculations (rightmost digits come first).
        for (int i = num1.length() - 1; i >= 0; i--) {
            for (int j = num2.length() - 1; j >= 0; j--) {
                // Multiply digits
                int digit1 = num1.charAt(i) - '0';
                int digit2 = num2.charAt(j) - '0';
                int mul = digit1 * digit2;
                
                // Find positions in the result array where to add the product.
                int posLow = i + j + 1;
                int posHigh = i + j;
                
                // Add multiplication result to the current position and handle carry
                int sum = mul + result[posLow];
                
                result[posLow] = sum % 10; // Set the current position to the remainder
                result[posHigh] += sum / 10; // Add the carry to the next higher position
            }
        }
        
        // Convert the result array into a string.
        StringBuilder product = new StringBuilder();
        for (int num : result) {
            // Skip leading zeros.
            if (!(product.length() == 0 && num == 0)) {
                product.append(num);
            }
        }
        
        // Return the product string.
        return product.length() == 0 ? "0" : product.toString();
    }
}
```