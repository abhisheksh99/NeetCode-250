# 518. Coin Change II

## Problem Statement
You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is guaranteed to fit into a signed 32-bit integer.

## Examples

### Example 1:
```
Input: amount = 5, coins = [1,2,5]
Output: 4
Explanation: There are four ways to make up the amount 5:
5 = 5
5 = 2 + 2 + 1
5 = 2 + 1 + 1 + 1
5 = 1 + 1 + 1 + 1 + 1
```

### Example 2:
```
Input: amount = 3, coins = [2]
Output: 0
Explanation: The amount 3 cannot be made up with just coins of denomination 2.
```

## Approach
We use dynamic programming to solve this problem:
1. Create a 1D DP array where `dp[i]` represents the number of ways to make amount `i`
2. Initialize `dp[0] = 1` because there's exactly one way to make amount 0 (using no coins)
3. For each coin, update the DP array from the coin's value up to the target amount
4. The recurrence relation is: `dp[j] += dp[j - coin]` where `coin` is the current coin value

## Solution Code
```java
class Solution {
    public int change(int amount, int[] coins) {
        // dp array to store the number of ways to make each amount
        int[] dp = new int[amount + 1];
        dp[0] = 1;  // There is one way to make the amount 0: use no coins

        // Update the dp array for each coin
        for (int coin : coins) {
            for (int j = coin; j <= amount; j++) {
                dp[j] += dp[j - coin];
            }
        }

        // The answer for the original amount
        return dp[amount];
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n × amount) - Where n is the number of coins
- **Space Complexity**: O(amount) - For the DP array

## Key Insights
1. **Order Matters**: By iterating through coins first and then amounts, we ensure combinations are counted only once in a specific order
2. **DP Initialization**: `dp[0] = 1` because there's exactly one way to make amount 0
3. **State Transition**: For each coin, we update the ways to make amounts from the coin's value up to the target
4. **Avoiding Duplicates**: The outer loop over coins ensures we don't count different orderings as separate combinations

## Edge Cases
1. Amount is 0 (should return 1)
2. No coins (should return 0 for any amount > 0)
3. Single coin that divides the amount
4. Single coin that doesn't divide the amount
5. Large amount with small coins (tests efficiency)

## Follow-up
How would you modify the solution if each coin could only be used once?