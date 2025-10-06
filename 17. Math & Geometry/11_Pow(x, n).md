# Pow(x, n)

**Difficulty:** Medium

## Problem Statement  
Implement `pow(x, n)`, which calculates `x` raised to the power `n` (i.e., `x^n`).

You need to handle positive, negative, and zero exponents efficiently.

---

## Example  
Input:  
x = 2.00000, n = 10


Output:  
1024.00000



Input:  
x = 2.10000, n = 3


Output: 9.26100


Input:  
x = 2.00000, n = -2


Output: 0.25000


Explanation:  
`2^-2 = 1/(2^2) = 1/4 = 0.25`

---

## Constraints  
- `-100.0 < x < 100.0`  
- `-2^31 <= n <= 2^31 - 1`  
- `n` is a 32-bit signed integer, which means it can be negative.

---

## Approach  
- Use **Exponentiation by Squaring** to efficiently compute powers in O(log n) time.  
- Handle the edge case when `n` is `Integer.MIN_VALUE` by converting `n` to a `long`.  
- If `n` is negative, invert `x` (take reciprocal) and negate `n`.  
- Iterate while `n` is greater than zero:  
  - If `n` is odd, multiply the result by the current base.  
  - Square the base and halve the exponent at each step.

---

## Time and Space Complexity  
- **Time Complexity:** O(log |n|), since exponent is divided by 2 every iteration.  
- **Space Complexity:** O(1), using constant extra space.

---

## Code

```java
public class Solution {
    public double myPow(double x, int n) {
        // Handle the base case where n is 0
        if (n == 0) return 1;
        
        // Convert n to long to handle edge case when n == Integer.MIN_VALUE
        long N = n;
        
        // If n is negative, we need to handle reciprocal of x
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        
        double result = 1;
        double currentProduct = x;
        
        // Exponentiation by squaring
        while (N > 0) {
            if (N % 2 == 1) {
                result *= currentProduct;
            }
            currentProduct *= currentProduct; // Square the base
            N /= 2; // Divide the exponent by 2
        }
        
        return result;
    }
}
```