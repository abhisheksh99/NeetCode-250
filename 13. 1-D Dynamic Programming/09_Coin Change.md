# 322. Coin Change

## Problem Statement
You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

### Examples

**Example 1:**
```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

**Example 2:**
```
Input: coins = [2], amount = 3
Output: -1
Explanation: You cannot make 3 with only coin of denomination 2.
```

**Example 3:**
```
Input: coins = [1], amount = 0
Output: 0
```

### Constraints:
- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 2^31 - 1`
- `0 <= amount <= 10^4`

## Solution
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int amt[] = new int[amount+1];
        
        Arrays.fill(amt, amount+1);
        
        amt[0] = 0;
        
        for(int i=1; i<=amount; i++){
            for(int j=0; j<coins.length; j++){
                if(i >= coins[j]){
                    amt[i] = Math.min(amt[i], 1+amt[i-coins[j]]);
                }
            }
        }
        
        if(amt[amount] < amount+1){
            return amt[amount];
        }
        return -1;
    }
}
```

## Approach
1. **Dynamic Programming (Bottom-up)**:
   - We create an array `amt` where `amt[i]` represents the minimum number of coins needed to make amount `i`.
   - Initialize all values in `amt` to `amount + 1` (a value larger than any possible minimum).
   - Set `amt[0] = 0` because 0 coins are needed to make amount 0.
   - For each amount from 1 to the target amount, try each coin denomination:
     - If the coin's value is less than or equal to the current amount, update `amt[i]` to be the minimum of its current value and `1 + amt[i - coin]`.
   - Finally, return `amt[amount]` if it's less than `amount + 1`, otherwise return -1.

## Complexity
- **Time Complexity**: O(n × amount)
  - Where n is the number of coins. We iterate through each coin for each amount from 1 to the target amount.
- **Space Complexity**: O(amount)
  - We use an array of size `amount + 1` to store the minimum coins needed for each amount.

## Edge Cases
- `coins = [1], amount = 0` → `0` (no coins needed)
- `coins = [2], amount = 3` → `-1` (impossible to make amount with given coins)
- `coins = [1, 2, 5], amount = 11` → `3` (5 + 5 + 1)
- `coins = [2], amount = 4` → `2` (2 + 2)
- `coins = [186, 419, 83, 408], amount = 6249` → `20` (larger test case)

## Key Insights
- The problem can be broken down into smaller subproblems, making it suitable for dynamic programming.
- By solving for smaller amounts first, we build up the solution to the target amount.
- The use of `amount + 1` as an initial value helps in identifying cases where it's impossible to make the target amount.
- The solution efficiently computes the minimum number of coins needed by leveraging previously computed results for smaller amounts.