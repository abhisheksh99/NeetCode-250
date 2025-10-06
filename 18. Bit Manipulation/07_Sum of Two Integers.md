# Sum of Two Integers Without Using '+' or '-'

## Problem Statement

Implement a function that adds two integers `a` and `b` without using the operators `+` or `-`.

---

## Examples

### Example 1
**Input:** `a = 1, b = 2`  
**Output:** `3`

### Example 2
**Input:** `a = -2, b = 3`  
**Output:** `1`

---

## Constraints

- `-1000 <= a, b <= 1000`
- You are **not allowed** to use the `+` or `-` operators.

---

## Approach

This solution uses **bitwise operations** to simulate the addition of two integers.

### Core Idea

1. **XOR (`^`)** gives the **sum** of two bits, but without the carry.
2. **AND (`&`) + shift left (`<<`)** gives the **carry**.
3. Repeat the process until there’s no carry left.

---

### Algorithm Steps

- While `b` is not zero:
  1. Calculate the **carry** using `(a & b) << 1`
  2. Update `a` to be the **sum without carry**: `a ^ b`
  3. Update `b` to be the **carry**
- When carry becomes `0`, `a` contains the final result.

---

## Time and Space Complexity

- **Time Complexity:** `O(1)` (since integers are 32-bit, loop runs at most 32 times)
- **Space Complexity:** `O(1)`

---

## Java Solution

```java
public class Solution {
    public int getSum(int a, int b) {
        while (b != 0) {
            int carry = (a & b) << 1; // Carry is ANDed and shifted left
            a ^= b;                   // Sum without carry
            b = carry;                // Carry becomes new b
        }
        return a;
    }
}
