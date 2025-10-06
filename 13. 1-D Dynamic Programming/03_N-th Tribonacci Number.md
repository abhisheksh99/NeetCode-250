# 1137. N-th Tribonacci Number

## Problem Statement
The Tribonacci sequence Tn is defined as follows: 

T0 = 0, T1 = 1, T2 = 1, and Tn+3 = Tn + Tn+1 + Tn+2 for n >= 0.

Given `n`, return the value of Tn.

### Examples

**Example 1:**
```
Input: n = 4
Output: 4
Explanation:
T_3 = 0 + 1 + 1 = 2
T_4 = 1 + 1 + 2 = 4
```

**Example 2:**
```
Input: n = 25
Output: 1389537
```

### Constraints:
- `0 <= n <= 37`
- The answer is guaranteed to fit within a 32-bit integer, i.e., `answer <= 2^31 - 1`.

## Solution
```java
public class Solution {
    public int tribonacci(int n) {
        // Base cases
        if (n <= 2) {
            return n == 0 ? 0 : 1;
        }

        // Initialize dp array where dp[i] represents the i-th Tribonacci number
        int[] dp = new int[n + 1];
        
        // Base values
        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 1;
        
        // Fill dp array using the recurrence relation
        for (int i = 3; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2] + dp[i - 3];
        }
        
        return dp[n];
    }
}
```

## Approach
1. **Dynamic Programming**:
   - We use a 1D array `dp` where `dp[i]` represents the i-th Tribonacci number.
   - The recurrence relation is given by `T(n) = T(n-1) + T(n-2) + T(n-3)` for n >= 3.
   - We initialize the base cases `dp[0] = 0`, `dp[1] = 1`, and `dp[2] = 1`.
   - We then iteratively compute each subsequent Tribonacci number up to n using the recurrence relation.

2. **Base Cases**:
   - If `n` is 0, return 0.
   - If `n` is 1 or 2, return 1.

## Complexity
- **Time Complexity**: O(n)
  - We perform a single pass through the array from index 3 to n, performing constant time operations in each iteration.
- **Space Complexity**: O(n)
  - We use an array of size n+1 to store intermediate results.
  - This can be optimized to O(1) by using three variables to store only the last three values instead of the entire array.

## Optimization (Space Complexity O(1))
```java
public int tribonacci(int n) {
    if (n == 0) return 0;
    if (n <= 2) return 1;
    
    int a = 0, b = 1, c = 1;
    for (int i = 3; i <= n; i++) {
        int d = a + b + c;
        a = b;
        b = c;
        c = d;
    }
    return c;
}
```

## Edge Cases
- `n = 0` → `0`
- `n = 1` → `1`
- `n = 2` → `1`
- `n = 4` → `4` (1 + 1 + 2)
- `n = 25` → `1389537`
- `n = 37` → `2082876103` (maximum value for n)

## Key Insights
- The problem is a variation of the Fibonacci sequence, where each term is the sum of the previous three terms instead of two.
- The dynamic programming approach efficiently computes the result by breaking down the problem into smaller subproblems and using their solutions to build up the solution to the larger problem.
- The space complexity can be optimized to O(1) by observing that we only need to keep track of the last three values at any point in the computation.