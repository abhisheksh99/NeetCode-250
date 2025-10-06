# Missing Number

## Problem Statement

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return the only number in the range that is missing from the array.

---

## Examples

### Example 1
**Input:** `nums = [3, 0, 1]`  
**Output:** `2`

### Example 2
**Input:** `nums = [0, 1]`  
**Output:** `2`

### Example 3
**Input:** `nums = [9,6,4,2,3,5,7,0,1]`  
**Output:** `8`

---

## Constraints

- `n == nums.length`
- `1 <= n <= 10⁴`
- `0 <= nums[i] <= n`
- All the numbers in `nums` are **unique**.

---

## Approach

We use the **mathematical formula** for the sum of the first `n` natural numbers:

\[
\text{Sum} = \frac{n \cdot (n + 1)}{2}
\]

### Steps:

1. Compute the **expected sum** of numbers from `0` to `n`.
2. Compute the **actual sum** of numbers in the array.
3. The **missing number** is the difference:
   \[
   \text{Missing} = \text{Expected Sum} - \text{Actual Sum}
   \]

---

## Time and Space Complexity

- **Time Complexity:** `O(n)`  
  We iterate once through the array to compute the actual sum.

- **Space Complexity:** `O(1)`  
  Only a few integer variables are used.

---

## Java Solution

```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;

        // Expected sum of numbers from 0 to n
        int expectedSum = n * (n + 1) / 2;

        // Actual sum of numbers in array
        int actualSum = 0;
        for (int num : nums) {
            actualSum += num;
        }

        // Missing number
        return expectedSum - actualSum;
    }
}
