# Hamming Weight Problem - Solution

## Problem Statement

The Hamming weight (also known as the population count) of a number is the number of `1` bits in its binary representation.

### Example 1:
**Input:** `n = 11`  
**Output:** `3`  
Explanation: The binary representation of `11` is `1011`, which has three `1` bits.

### Example 2:
**Input:** `n = 128`  
**Output:** `1`  
Explanation: The binary representation of `128` is `10000000`, which has only one `1` bit.

### Example 3:
**Input:** `n = 255`  
**Output:** `8`  
Explanation: The binary representation of `255` is `11111111`, which has eight `1` bits.

### Constraints:
- `n` is a 32-bit signed integer.

## Approach

To count the number of `1` bits in the binary representation of an integer `n`, we can use the following efficient approach:

1. **Bitwise Operation (`n & (n - 1)`):**
   - The operation `n & (n - 1)` removes the rightmost `1` bit from the number `n`. This is a common technique for counting the number of set bits (1s) in a number's binary representation.
   - Repeating this operation until `n` becomes `0` will remove all the `1` bits from `n`, and the number of iterations will be equal to the number of `1` bits in `n`.

2. **Algorithm:**
   - Initialize a result variable `res` to 0.
   - While `n` is not zero:
     - Apply `n = n & (n - 1)` to remove the rightmost `1` bit.
     - Increment the result variable `res`.
   - When `n` becomes zero, return the final result `res`, which will contain the count of `1` bits in the binary representation of `n`.

### Time Complexity:
- **O(k)** where `k` is the number of bits in the integer. Since the number of bits is constant (typically 32 or 64), the time complexity is effectively **O(1)**.

### Space Complexity:
- **O(1)**, since we only use a constant amount of extra space.

## Solution Code

```java
class Solution {
    public int hammingWeight(int n) {
        int res = 0;

        while (n != 0) {
            // Remove the rightmost 1-bit using n & (n - 1)
            n = n & (n - 1);
            res++;
        }

        return res;
    }
}
