# Count Bits Problem - Solution

## Problem Statement

Given an integer `n`, return an array `ans` where each element `ans[i]` is the number of `1` bits (Hamming weight) in the binary representation of the integer `i`, for all `i` in the range from `0` to `n`.

### Example 1:
**Input:** `n = 5`  
**Output:** `[0, 1, 1, 2, 1, 2]`  
Explanation:  
- `0` in binary is `0` (0 ones)  
- `1` in binary is `1` (1 one)  
- `2` in binary is `10` (1 one)  
- `3` in binary is `11` (2 ones)  
- `4` in binary is `100` (1 one)  
- `5` in binary is `101` (2 ones)

### Example 2:
**Input:** `n = 2`  
**Output:** `[0, 1, 1]`  
Explanation:  
- `0` in binary is `0` (0 ones)  
- `1` in binary is `1` (1 one)  
- `2` in binary is `10` (1 one)

### Constraints:
- `0 <= n <= 10^5`

## Approach

The solution leverages dynamic programming with bit manipulation to efficiently compute the number of `1` bits for all integers from `0` to `n`.

### Formula:
For each integer `i`, the number of `1` bits can be computed using:
- `ans[i] = ans[i & (i - 1)] + 1`

Where:
- `i & (i - 1)` removes the rightmost set bit (1) from `i`.
- The result is the number of `1` bits in the binary representation of `i`.

### Steps:
1. **Initialization**: Create an array `ans` of size `n + 1` to store the results.
2. **Base Case**: `ans[0] = 0` (because `0` has no `1` bits).
3. **Loop**: For each `i` from `1` to `n`, compute the number of `1` bits using the formula:
   - `ans[i] = ans[i & (i - 1)] + 1`
4. **Return**: Return the array `ans` containing the number of `1` bits for each number from `0` to `n`.

### Time Complexity:
- **O(n)**: The loop iterates through numbers from `1` to `n`, and each operation inside the loop takes constant time.

### Space Complexity:
- **O(n)**: We store the results for all numbers from `0` to `n` in the `ans` array.

## Solution Code

```java
class Solution {
    public int[] countBits(int n) {
        int[] ans = new int[n + 1];  // Array to store the number of 1-bits for each number from 0 to n
        ans[0] = 0;  // Base case: number of 1-bits in 0 is 0

        // Loop through all numbers from 1 to n
        for (int i = 1; i <= n; i++) {
            // Apply the formula: ans[i] = ans[i & (i - 1)] + 1
            ans[i] = ans[i & (i - 1)] + 1;
        }

        return ans;  // Return the result array
    }
}
