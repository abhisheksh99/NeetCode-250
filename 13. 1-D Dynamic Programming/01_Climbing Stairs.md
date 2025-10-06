# 70. Climbing Stairs

## Problem Statement
You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

### Examples

**Example 1:**
```
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

**Example 2:**
```
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

### Constraints:
- `1 <= n <= 45`

## Solution
```java
class Solution {
    public int climbStairs(int n) {
        if (n == 1) {
            return 1;
        }
        // dp[i] represents the number of ways to reach step i
        int[] dp = new int[n + 1];
        
        // Base cases
        dp[1] = 1; // Only 1 way to reach step 1
        dp[2] = 2; // Two ways: (1+1) or (2)
        
        // Fill dp array using the recurrence relation
        for (int i = 3; i <= n; i++) {
            // The number of ways to reach step i is the sum of ways to reach step i-1 and step i-2
            // Because you can take a single step from i-1 or a double step from i-2
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        
        return dp[n];
    }
}
```

## Approach
1. **Dynamic Programming (Bottom-up)**:
   - We use a 1D array `dp` where `dp[i]` represents the number of distinct ways to reach the ith step.
   - The key observation is that the number of ways to reach step `i` is equal to the sum of ways to reach step `i-1` and step `i-2`.
   - This is because from step `i-1`, you can take a single step to reach `i`, and from step `i-2`, you can take a double step to reach `i`.

2. **Base Cases**:
   - `dp[1] = 1`: Only one way to reach the first step (take a single step of 1).
   - `dp[2] = 2`: Two ways to reach the second step (1+1 or 2).

3. **Filling the DP Array**:
   - For each step `i` from 3 to n, compute `dp[i] = dp[i-1] + dp[i-2]`.
   - The final answer is `dp[n]`.

## Complexity
- **Time Complexity**: O(n)
  - We iterate through the array once from 3 to n, performing constant time operations in each iteration.
- **Space Complexity**: O(n)
  - We use an array of size n+1 to store intermediate results.
  - This can be optimized to O(1) by using variables to store only the last two values instead of the entire array.

## Optimization (Space Complexity O(1))
```java
public int climbStairs(int n) {
    if (n == 1) return 1;
    
    int first = 1;  // dp[1]
    int second = 2; // dp[2]
    
    for (int i = 3; i <= n; i++) {
        int third = first + second;
        first = second;
        second = third;
    }
    
    return second;
}
```

## Edge Cases
- `n = 1`: Only 1 way to climb (take 1 step)
- `n = 2`: 2 ways (1+1 or 2)
- `n = 3`: 3 ways (1+1+1, 1+2, 2+1)
- `n = 45`: Maximum constraint value, should return 1,836,311,903

## Key Insights
- The problem follows the Fibonacci sequence, where each term is the sum of the two preceding ones.
- The number of ways to climb `n` stairs is the (n+1)th Fibonacci number if we consider the Fibonacci sequence as 1, 1, 2, 3, 5, ...
- The dynamic programming approach efficiently computes the result by breaking down the problem into smaller subproblems and using their solutions to build up the solution to the larger problem.