# Reverse Integer

## Problem Statement

Given a signed 32-bit integer `x`, return `x` with its digits reversed. If reversing `x` causes the value to go outside the signed 32-bit integer range `[-2³¹, 2³¹ - 1]`, return `0`.

> **Note:** Do not store 64-bit integers (such as `long`) to handle overflow.

---

## Examples

### Example 1
**Input:** `x = 123`  
**Output:** `321`

### Example 2
**Input:** `x = -123`  
**Output:** `-321`

### Example 3
**Input:** `x = 120`  
**Output:** `21`

### Example 4
**Input:** `x = 0`  
**Output:** `0`

---

## Constraints

- `-2³¹ <= x <= 2³¹ - 1`
- The solution **must not use** 64-bit integers (i.e., no `long`).

---

## Approach

The solution uses **mathematics** and **careful overflow detection**:

### Steps:
1. Initialize a variable `rev` to store the reversed number.
2. Loop while `x` is not `0`:
   - Extract the **last digit** of `x`: `pop = x % 10`
   - Reduce `x`: `x /= 10`
   - **Check for overflow or underflow**:
     - If `rev > Integer.MAX_VALUE / 10` OR  
       `rev == Integer.MAX_VALUE / 10 && pop > 7` → Overflow
     - If `rev < Integer.MIN_VALUE / 10` OR  
       `rev == Integer.MIN_VALUE / 10 && pop < -8` → Underflow
   - Append the digit: `rev = rev * 10 + pop`
3. Return `rev`.

---

## Java Solution

```java
class Solution {
    public int reverse(int x) {
        int rev = 0;
        
        while (x != 0) {
            // Extract the last digit
            int pop = x % 10;
            x /= 10;
            
            // Check for overflow
            if (rev > Integer.MAX_VALUE / 10 || (rev == Integer.MAX_VALUE / 10 && pop > 7)) {
                return 0;
            }
            if (rev < Integer.MIN_VALUE / 10 || (rev == Integer.MIN_VALUE / 10 && pop < -8)) {
                return 0;
            }
            
            // Append the digit
            rev = rev * 10 + pop;
        }
        
        return rev;
    }
}
