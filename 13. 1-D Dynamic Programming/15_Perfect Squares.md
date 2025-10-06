# 279. Perfect Squares

## Problem Statement
Given an integer `n`, return the least number of perfect square numbers that sum to `n`.

A perfect square is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, 1, 4, 9, and 16 are perfect squares while 3 and 11 are not.

### Examples

**Example 1:**
```
Input: n = 12
Output: 3 
Explanation: 12 = 4 + 4 + 4.
```

**Example 2:**
```
Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
```

### Constraints:
- `1 <= n <= 10^4`

## Solution
```java
public class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        Arrays.fill(dp, n);
        dp[0] = 0;

        for (int target = 1; target <= n; target++) {
            for (int s = 1; s * s <= target; s++) {
                dp[target] = Math.min(dp[target], 1 + dp[target - s * s]);
            }
        }

        return dp[n];
    }
}
```

## Approach
1. **Dynamic Programming Initialization**:
   - Create a `dp` array of size `n+1` initialized with `n` (the maximum possible number of squares needed).
   - Set `dp[0] = 0` because 0 can be represented by 0 squares.

2. **DP Array Update**:
   - For each target number from 1 to `n`:
     - Try every perfect square number `s*s` that is less than or equal to the target.
     - Update `dp[target]` to be the minimum of its current value and `1 + dp[target - s*s]`.
     - This represents using one more square number (`s*s`) to reach the target.

3. **Result**:
   - The value at `dp[n]` will contain the minimum number of perfect squares that sum to `n`.

## Complexity
- **Time Complexity**: O(n * √n)
  - We iterate through each number up to `n`, and for each number, we check up to √n perfect squares.
- **Space Complexity**: O(n)
  - We use an array of size `n+1` to store intermediate results.

## Edge Cases
- `n = 1` → `1` (1 = 1)
- `n = 2` → `2` (1 + 1)
- `n = 4` → `1` (4 is a perfect square itself)
- `n = 12` → `3` (4 + 4 + 4)
- `n = 13` → `2` (4 + 9)

## Key Insights
- The problem can be efficiently solved using dynamic programming by breaking it down into smaller subproblems.
- Each number `i` can be represented as `i = j*j + k`, where `j*j` is a perfect square and `k` is the remaining value.
- We use the `dp` array to store the minimum number of perfect squares needed for each number up to `n`.
- The solution builds up the answer from smaller numbers to larger numbers, reusing previously computed results.
- The inner loop only needs to check perfect squares up to the square root of the current target, which optimizes the solution.