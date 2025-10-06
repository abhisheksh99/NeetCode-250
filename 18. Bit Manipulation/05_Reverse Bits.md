# Reverse Bits Problem - Solution

## Problem Statement

Reverse bits of a given 32-bit unsigned integer.

### Example 1:
**Input:**  
`n = 00000010100101000001111010011100`  
**Output:**  
`964176192` (which is `00111001011110000010100101000000` in binary)

### Example 2:
**Input:**  
`n = 11111111111111111111111111111101`  
**Output:**  
`3221225471` (which is `10111111111111111111111111111111` in binary)

### Note:
- The input is treated as an unsigned 32-bit integer.
- The function returns the result as a signed integer (due to Java's lack of unsigned int), but the bit pattern is correct.

---

## Approach

To reverse the bits of a 32-bit unsigned integer:

1. Initialize a result variable `reverse` to 0.
2. Iterate over all 32 bits (from bit 0 to bit 31).
3. In each iteration:
   - Left-shift `reverse` by 1 to make space for the new bit.
   - Extract the least significant bit (LSB) of `n` using `n & 1` and append it to `reverse` using bitwise OR (`|`).
   - Right-shift `n` by 1 to process the next bit.
4. Return `reverse`.

---

## Time and Space Complexity

- **Time Complexity:** O(1) — Always runs 32 iterations, regardless of input.
- **Space Complexity:** O(1) — Uses constant space.

---

## Solution Code

```java
class Solution {
    public int reverseBits(int n) {
        int reverse = 0;
        for (int i = 0; i < 32; i++) {
            reverse = reverse << 1;           // Shift left to make room for the next bit
            reverse = reverse | (n & 1);      // Add the least significant bit of n
            n = n >> 1;                       // Shift n right to get the next bit
        }
        return reverse;
    }
}
