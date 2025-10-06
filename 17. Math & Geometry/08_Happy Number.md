# Happy Number

**Difficulty:** Easy

## Problem Statement  
Write an algorithm to determine if a number `n` is a "happy number".

A happy number is defined by the following process:  
- Starting with any positive integer, replace the number by the sum of the squares of its digits.  
- Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.  
- Those numbers for which this process ends in 1 are happy numbers.

Return `true` if `n` is a happy number, and `false` if it is not.

---

## Example  
Input: `n = 19`  
Output: `true`  
Explanation:  
1² + 9² = 82
8² + 2² = 68
6² + 8² = 100
1² + 0² + 0² = 1


Since the process ends in 1, 19 is a happy number.

---

## Constraints  
- `1 <= n <= 2^31 - 1`

---

## Approach  
- Use a `Set` to track numbers seen during the process to detect cycles.  
- Iteratively replace the number by the sum of the squares of its digits.  
- If the number becomes 1, return `true`.  
- If the number repeats (already in the set), it means a cycle exists and return `false`.

---

## Time and Space Complexity  
- **Time Complexity:** O(log n), since each step reduces the number and involves summing squares of digits.  
- **Space Complexity:** O(log n), for the set storing numbers in the worst case.

---

## Code

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public static boolean isHappy(int n) {
        Set<Integer> seenNumbers = new HashSet<>();

        while (n != 1 && !seenNumbers.contains(n)) {
            seenNumbers.add(n);
            n = getSumOfSquares(n);
        }

        return n == 1;
    }

    private static int getSumOfSquares(int n) {
        int sum = 0;
        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }
        return sum;
    }
}
```