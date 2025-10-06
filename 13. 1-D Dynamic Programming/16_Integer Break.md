# 343. Integer Break

## Problem Statement
Given an integer `n`, break it into the sum of `k` positive integers, where `k >= 2`, and maximize the product of those integers.

Return the maximum product you can get.

### Examples

**Example 1:**
```
Input: n = 2
Output: 1
Explanation: 2 = 1 + 1, 1 × 1 = 1.
```

**Example 2:**
```
Input: n = 10
Output: 36
Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.
```

### Constraints:
- `2 <= n <= 58`

## Solution
```java
public class Solution {
    public int integerBreak(int n) {
        int[] dp = new int[n + 1];
        dp[1] = 1;

        for (int num = 2; num <= n; num++) {
            dp[num] = (num == n) ? 0 : num;
            for (int i = 1; i < num; i++) {
                dp[num] = Math.max(dp[num], dp[i] * dp[num - i]);
            }
        }

        return dp[n];
    }
}
```

## Approach
1. **Dynamic Programming Initialization**:
   - Create a `dp` array where `dp[i]` represents the maximum product of integers that sum up to `i`.
   - Initialize `dp[1] = 1` since the maximum product of numbers that sum to 1 is 1 (though k >= 2, this is a base case).

2. **DP Array Update**:
   - For each number `num` from 2 to `n`:
     - If `num` is not the original number `n`, initialize `dp[num]` to `num` (the number itself is a valid product if we don't break it further).
     - For `num == n`, initialize `dp[num]` to 0 since we must break it into at least two positive integers.
     - For each possible split `i` (from 1 to `num-1`):
       - Update `dp[num]` to be the maximum of its current value and the product of `dp[i]` and `dp[num - i]`.

3. **Result**:
   - The value at `dp[n]` will contain the maximum product of integers that sum to `n`.

## Complexity
- **Time Complexity**: O(n²)
  - We have nested loops: the outer loop runs from 2 to `n`, and the inner loop runs up to `num-1` for each `num`.
- **Space Complexity**: O(n)
  - We use an array of size `n + 1` to store intermediate results.

## Edge Cases
- `n = 2` → `1` (1 + 1 = 2, 1 × 1 = 1)
- `n = 3` → `2` (1 + 2 = 3, 1 × 2 = 2)
- `n = 4` → `4` (2 + 2 = 4, 2 × 2 = 4)
- `n = 10` → `36` (3 + 3 + 4 = 10, 3 × 3 × 4 = 36)

## Key Insights
- The problem can be efficiently solved using dynamic programming by breaking it down into smaller subproblems.
- For each number `i` from 2 to `n`, we consider all possible ways to split `i` into two parts and take the maximum product.
- The key observation is that the maximum product for `i` can be obtained by taking the maximum of all possible products of two numbers that sum to `i`.
- The solution builds up the answer from smaller numbers to larger numbers, reusing previously computed results.
- The special case for `num == n` ensures that we break the number into at least two parts when computing the final result.