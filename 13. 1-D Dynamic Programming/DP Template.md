# Dynamic Programming (DP) Template

## 1. General Approach
Dynamic Programming is a method for solving complex problems by breaking them down into simpler subproblems. It is particularly useful for optimization problems where you need to find the minimum/maximum/longest/shortest of something.

### When to Use DP
- Problems that ask for maximum/minimum of something
- Problems that can be broken down into overlapping subproblems
- Problems that exhibit optimal substructure (optimal solution can be constructed from optimal solutions of subproblems)

## 2. Top-Down (Memoization) Approach
### Template
```java
class Solution {
    // Memoization table (can be HashMap or array)
    private Map<Integer, Integer> memo = new HashMap<>();
    
    public int solve(int[] nums) {
        return dp(nums, 0);
    }
    
    private int dp(int[] nums, int i) {
        // Base cases
        if (i >= nums.length) {
            return 0;
        }
        
        // Check if already computed
        if (memo.containsKey(i)) {
            return memo.get(i);
        }
        
        // Recurrence relation
        int result = Math.min(
            dp(nums, i + 1) + nums[i],
            dp(nums, i + 2) + nums[i + 1]
        );
        
        // Memoize and return
        memo.put(i, result);
        return result;
    }
}
```

### When to Use Top-Down
- When the problem has a clear recursive structure
- When not all subproblems need to be solved
- When the state space is sparse

## 3. Bottom-Up (Tabulation) Approach
### Template
```java
class Solution {
    public int solve(int[] nums) {
        int n = nums.length;
        // DP table initialization
        int[] dp = new int[n + 1];
        
        // Base cases
        dp[0] = 0;
        dp[1] = nums[0];
        
        // Fill the DP table
        for (int i = 2; i <= n; i++) {
            // Recurrence relation
            dp[i] = Math.min(
                dp[i - 1] + nums[i - 1],
                dp[i - 2] + nums[i - 1]
            );
        }
        
        return dp[n];
    }
}
```

### When to Use Bottom-Up
- When you need to compute all subproblems
- When you want to optimize for space (can often be optimized to O(1) space)
- When the problem has a natural iterative structure

## 4. Space Optimization
### From O(n) to O(1) Space
```java
class Solution {
    public int solve(int[] nums) {
        int n = nums.length;
        if (n == 0) return 0;
        if (n == 1) return nums[0];
        
        int prev1 = nums[0];
        int prev2 = Math.min(nums[0], nums[1]);
        
        for (int i = 2; i < n; i++) {
            int current = Math.min(prev1, prev2) + nums[i];
            prev1 = prev2;
            prev2 = current;
        }
        
        return Math.min(prev1, prev2);
    }
}
```

## 5. Common DP Patterns
1. **Fibonacci Sequence**
   - dp[i] = dp[i-1] + dp[i-2]

2. **0/1 Knapsack**
   - dp[i][j] = max(dp[i-1][j], dp[i-1][j-w] + v)

3. **Unbounded Knapsack**
   - dp[i] = min(dp[i], dp[i-coin] + 1)

4. **Longest Increasing Subsequence**
   - dp[i] = max(dp[i], dp[j] + 1) for all j < i and nums[j] < nums[i]

5. **Longest Common Subsequence**
   - if (s1[i-1] == s2[j-1]) dp[i][j] = 1 + dp[i-1][j-1]
   - else dp[i][j] = max(dp[i-1][j], dp[i][j-1])

## 6. Problem Identification
- **Count the number of ways to...** → DP
- **Find the minimum/maximum of...** → DP
- **Is it possible to reach...** → DP
- **What's the longest/shortest...** → DP

## 7. Tips and Tricks
1. Always start with a recursive solution and then optimize it with DP
2. Draw the recursion tree to identify overlapping subproblems
3. Start with a 1D or 2D DP array based on the number of changing parameters
4. Think about the base cases carefully
5. The order of iteration matters in bottom-up DP

## 8. Common Mistakes
- Forgetting to handle base cases
- Incorrectly defining the state
- Not considering all possible transitions
- Using the wrong order of iteration
- Not initializing the DP table properly