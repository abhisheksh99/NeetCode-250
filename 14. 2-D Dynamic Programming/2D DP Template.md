# 2D Dynamic Programming Template

## 1. When to Use 2D DP
- Problems involving 2D grids or matrices
- String matching problems with two strings (e.g., LCS, Edit Distance)
- Problems with two changing parameters
- Knapsack problems with item constraints

## 2. Top-Down (Memoization) Approach
### Time: O(mn), Space: O(mn) + Recursive Stack
```java
class Solution {
    private int[][] memo;
    
    public int solve2D(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        memo = new int[m][n];
        for (int[] row : memo) {
            Arrays.fill(row, -1);
        }
        return dp(grid, 0, 0);
    }
    
    private int dp(int[][] grid, int i, int j) {
        // Base cases
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length) {
            return 0; // or appropriate base value
        }
        
        // Check if already computed
        if (memo[i][j] != -1) {
            return memo[i][j];
        }
        
        // Recurrence relation
        int result = grid[i][j] + Math.min(
            dp(grid, i + 1, j),    // Down
            dp(grid, i, j + 1)     // Right
        );
        
        // Memoize and return
        memo[i][j] = result;
        return result;
    }
}
```

## 3. Bottom-Up (Tabulation) Approach
### Time: O(mn), Space: O(mn)
```java
class Solution {
    public int solve2D(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] dp = new int[m][n];
        
        // Base case
        dp[0][0] = grid[0][0];
        
        // Fill first row
        for (int j = 1; j < n; j++) {
            dp[0][j] = dp[0][j-1] + grid[0][j];
        }
        
        // Fill first column
        for (int i = 1; i < m; i++) {
            dp[i][0] = dp[i-1][0] + grid[i][0];
        }
        
        // Fill the rest
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = grid[i][j] + Math.min(dp[i-1][j], dp[i][j-1]);
            }
        }
        
        return dp[m-1][n-1];
    }
}
```

## 4. Space Optimization (1D Array)
### Time: O(mn), Space: O(n)
```java
class Solution {
    public int solve2D(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[] dp = new int[n];
        
        // Initialize first row
        dp[0] = grid[0][0];
        for (int j = 1; j < n; j++) {
            dp[j] = dp[j-1] + grid[0][j];
        }
        
        // Fill remaining rows
        for (int i = 1; i < m; i++) {
            dp[0] += grid[i][0]; // First column
            for (int j = 1; j < n; j++) {
                dp[j] = grid[i][j] + Math.min(dp[j-1], dp[j]);
            }
        }
        
        return dp[n-1];
    }
}
```

## 5. Common 2D DP Patterns

### 5.1 Grid Traversal (Min/Max Path Sum)
```java
// dp[i][j] = grid[i][j] + Math.min(dp[i-1][j], dp[i][j-1])
```

### 5.2 Longest Common Subsequence (LCS)
```java
// if (text1.charAt(i-1) == text2.charAt(j-1))
//     dp[i][j] = 1 + dp[i-1][j-1]
// else
//     dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1])
```

### 5.3 Edit Distance
```java
// if (word1.charAt(i-1) == word2.charAt(j-1))
//     dp[i][j] = dp[i-1][j-1]
// else
//     dp[i][j] = 1 + Math.min(dp[i-1][j], Math.min(dp[i][j-1], dp[i-1][j-1]))
```

### 5.4 0/1 Knapsack
```java
// if (wt[i-1] > j)
//     dp[i][j] = dp[i-1][j]
// else
//     dp[i][j] = Math.max(val[i-1] + dp[i-1][j-wt[i-1]], dp[i-1][j])
```

## 6. Tips for 2D DP
1. **State Definition**: Clearly define what dp[i][j] represents
2. **Initialization**: Pay special attention to first row and column
3. **State Transition**: Think about how current state relates to previous states
4. **Order of Iteration**: Row-wise or column-wise? Depends on state transitions
5. **Space Optimization**: Can you reduce from O(mn) to O(n) or O(1)?

## 7. Common Mistakes
- Off-by-one errors in indices
- Incorrect base case initialization
- Wrong order of iteration
- Forgetting to handle edge cases (empty grid, single row/column)
- Not considering all possible transitions

## 8. Time and Space Complexity
| Approach | Time | Space |
|----------|------|-------|
| Top-Down | O(mn) | O(mn) + Recursive Stack |
| Bottom-Up | O(mn) | O(mn) |
| Optimized | O(mn) | O(n) or O(1) |

## 9. When to Use Which Approach
- **Top-Down**: When not all states need to be computed
- **Bottom-Up**: When you need to compute all states anyway
- **Space Optimized**: When space is a constraint

## 10. Template for New Problems
1. Define the state dp[i][j]
2. Write the base cases
3. Write the recurrence relation
4. Choose iteration order
5. Implement and test edge cases