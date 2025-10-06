# 1406. Stone Game III

## Problem Statement
Alice and Bob continue their games with piles of stones. There are several stones arranged in a row, and each stone has an associated value which is an integer given in the array `stoneValue`.

Alice and Bob take turns, with Alice starting first. On each player's turn, that player can take 1, 2, or 3 stones from the first remaining stones in the row.

The score of each player is the sum of the values of the stones taken. The score of each player is 0 initially.

The objective of the game is to end with the highest score, and the winner is the player with the highest score and there could be a tie. The game continues until all the stones have been taken.

Assume both players play optimally, return "Alice" if Alice wins, "Bob" if Bob wins, or "Tie" if they end the game with the same score.

### Examples

**Example 1:**
```
Input: stoneValue = [1,2,3,7]
Output: "Bob"
Explanation: Alice will always lose. Her best move would be to take three piles and the score become 6. Now the score of Bob is 7 and Bob wins.
```

**Example 2:**
```
Input: stoneValue = [1,2,3,-9]
Output: "Alice"
Explanation: Alice must choose all the three piles at the first move to win and leave Bob with negative score.
```

### Constraints:
- `1 <= stoneValue.length <= 5 * 10^4`
- `-1000 <= stoneValue[i] <= 1000`

## Solution
```java
public class Solution {
    public String stoneGameIII(int[] stoneValue) {
        int n = stoneValue.length;
        int[] dp = new int[n + 1];
        Arrays.fill(dp, Integer.MIN_VALUE);
        dp[n] = 0;

        for (int i = n - 1; i >= 0; i--) {
            int total = 0;
            dp[i] = Integer.MIN_VALUE;
            for (int j = i; j < Math.min(i + 3, n); j++) {
                total += stoneValue[j];
                dp[i] = Math.max(dp[i], total - dp[j + 1]);
            }
        }

        int result = dp[0];
        if (result == 0) return "Tie";
        return result > 0 ? "Alice" : "Bob";
    }
}
```

## Approach
1. **Dynamic Programming Initialization**:
   - Create a `dp` array where `dp[i]` represents the maximum relative score difference the current player can achieve starting from position `i`.
   - Initialize all values to `Integer.MIN_VALUE` to represent uncomputed states.
   - Set `dp[n] = 0` since there are no stones left to take at the end.

2. **DP Array Update**:
   - Iterate backwards from the end of the array to the beginning.
   - For each position `i`, consider taking 1, 2, or 3 stones:
     - Calculate the total value of stones taken in the current move.
     - Update `dp[i]` to be the maximum of its current value and `total - dp[j + 1]`, where `j` is the end of the current move.
     - This represents the current player taking the stones and then the opponent being left with the remaining stones.

3. **Result Determination**:
   - The value at `dp[0]` represents the maximum relative score difference Alice can achieve over Bob.
   - If `dp[0]` is 0, the game is a tie.
   - If `dp[0]` is positive, Alice wins; otherwise, Bob wins.

## Complexity
- **Time Complexity**: O(n)
  - We iterate through the array once, and for each position, we check at most 3 positions ahead.
- **Space Complexity**: O(n)
  - We use an array of size `n + 1` to store intermediate results.

## Edge Cases
- `stoneValue = [1,2,3,7]` → `"Bob"` (Alice takes 1+2+3=6, Bob takes 7)
- `stoneValue = [1,2,3,-9]` → `"Alice"` (Alice takes all to leave Bob with negative score)
- `stoneValue = [-1,-2,-3]` → `"Tie"` (Both players end with negative scores)
- `stoneValue = [1,2,3,6]` → `"Tie"` (Both players can end with equal scores)

## Key Insights
- The problem is a variation of the classic game theory problem and can be efficiently solved using dynamic programming.
- The key insight is to work backwards from the end of the array, calculating the optimal move at each position based on future positions.
- The solution uses the concept of relative score difference to determine the winner, which simplifies the state representation.
- The approach ensures that both players play optimally by always choosing the move that maximizes their relative advantage.