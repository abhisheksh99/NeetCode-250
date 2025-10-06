# 254. Factor Combinations

## Problem Statement
Numbers can be regarded as the product of their factors. Given an integer `n`, return all possible combinations of its factors. You may return the answer in any order.

**Note** that the factors should be in the range `[2, n - 1]` and the combinations should be in non-decreasing order.

## Examples

### Example 1:
```
Input: n = 1
Output: []
Explanation: 1 has no factors, so we return an empty list.
```

### Example 2:
```
Input: n = 12
Output: [[2,6],[2,2,3],[3,4]]
Explanation: 
2 × 6 = 12
2 × 2 × 3 = 12
3 × 4 = 12
```

### Example 3:
```
Input: n = 37
Output: []
Explanation: 37 is a prime number, so we return an empty list.
```

## Approach
We can solve this problem using backtracking:
1. Start with the smallest factor (2) and try to divide `n` by it.
2. If it divides evenly, we have a valid factor. We then recursively find factors of the quotient that are greater than or equal to the current factor.
3. Continue this process until the quotient becomes 1, at which point we add the current combination to the result.
4. To avoid duplicate combinations, we ensure that factors are in non-decreasing order.

## Solution Code
```java
import java.util.*;

class Solution {
    public List<List<Integer>> getFactors(int n) {
        List<List<Integer>> result = new ArrayList<>();
        if (n <= 3) {
            return result;  // No valid factors for n <= 3
        }
        backtrack(n, 2, new ArrayList<>(), result);
        return result;
    }
    
    private void backtrack(int n, int start, List<Integer> current, List<List<Integer>> result) {
        // Base case: if n becomes 1 and we have more than one factor in current list
        if (n == 1) {
            if (current.size() > 1) {
                result.add(new ArrayList<>(current));
            }
            return;
        }
        
        // Try all possible factors starting from 'start' to sqrt(n)
        for (int i = start; i <= Math.sqrt(n); i++) {
            if (n % i == 0) {
                // Add current factor
                current.add(i);
                // Continue with the quotient and same start factor
                backtrack(n / i, i, current, result);
                // Backtrack
                current.remove(current.size() - 1);
            }
        }
        
        // Add the remaining n as the last factor if it's greater than or equal to start
        if (n >= start) {
            current.add(n);
            backtrack(1, n, current, result);
            current.remove(current.size() - 1);
        }
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n^log n) - In the worst case, we might have a complete binary tree of factors.
- **Space Complexity**: O(log n) - The maximum depth of the recursion stack is log n (base 2).

## Key Insights
1. **Backtracking**: We use backtracking to explore all possible factor combinations.
2. **Pruning**: We only check factors up to the square root of the current number to avoid duplicate combinations.
3. **Non-decreasing Order**: By starting the next factor search from the current factor, we ensure the factors are in non-decreasing order.
4. **Base Case Handling**: We ensure that we only add valid combinations (at least two factors).

## Edge Cases
1. n = 1 (should return empty list)
2. n is a prime number (should return empty list)
3. n is a perfect square
4. n has many factors (e.g., 32, 64)
5. Large values of n (tests efficiency)

## Follow-up
How would you modify the solution to include the number itself as a valid combination? For example, for n = 12, the output would include [12] as well.