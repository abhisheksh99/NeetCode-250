# 309. Best Time to Buy and Sell Stock with Cooldown

## Problem Statement
You are given an array `prices` where `prices[i]` is the price of a given stock on the `i-th` day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

- After you sell your stock, you cannot buy stock on the next day (i.e., cooldown for one day).
- You cannot engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

## Examples

### Example 1:
```
Input: prices = [1,2,3,0,2]
Output: 3
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```

### Example 2:
```
Input: prices = [1]
Output: 0
```

## Approach
We use a state machine approach with dynamic programming to track three states:
1. **Hold (buy)**: Maximum profit when holding a stock
2. **Sold (sell)**: Maximum profit after selling the stock (must cooldown next day)
3. **Rest (cooldown)**: Maximum profit when in cooldown or doing nothing

The transitions between these states are:
- From rest or cooldown, we can choose to buy (enter hold state)
- From hold, we can choose to sell (enter sold state)
- From sold, we must enter cooldown (rest state)

## Solution Code
```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length <= 1) return 0;

        int n = prices.length;
        int[] hold = new int[n];    // Maximum profit when holding a stock
        int[] sold = new int[n];    // Maximum profit after selling the stock
        int[] rest = new int[n];    // Maximum profit when in cooldown or doing nothing

        // Base cases
        hold[0] = -prices[0];  // Buying on the first day
        sold[0] = 0;           // Cannot sell on the first day
        rest[0] = 0;           // No profit on the first day if we rest

        for (int i = 1; i < n; i++) {
            // Can hold from previous hold or buy from rest (cooldown)
            hold[i] = Math.max(hold[i-1], rest[i-1] - prices[i]);
            
            // Can sell from hold state
            sold[i] = Math.max(sold[i-1], hold[i-1] + prices[i]);
            
            // Can rest from previous rest or after selling
            rest[i] = Math.max(rest[i-1], sold[i-1]);
        }

        // The maximum profit will be the maximum of sold or rest on the last day
        return Math.max(sold[n-1], rest[n-1]);
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n) - Single pass through the prices array
- **Space Complexity**: O(n) - Three arrays of size n (can be optimized to O(1))

## Space Optimization
We can optimize the space to O(1) by using variables instead of arrays:
```java
public int maxProfit(int[] prices) {
    if (prices == null || prices.length <= 1) return 0;
    
    int hold = -prices[0];  // Buy on first day
    int sold = 0;           // Cannot sell on first day
    int rest = 0;           // No profit if we do nothing
    
    for (int i = 1; i < prices.length; i++) {
        int prevHold = hold;
        int prevSold = sold;
        int prevRest = rest;
        
        hold = Math.max(prevHold, prevRest - prices[i]);
        sold = prevHold + prices[i];
        rest = Math.max(prevRest, prevSold);
    }
    
    return Math.max(sold, rest);
}
```

## Key Insights
1. **State Transitions**: The key is to model the three possible states and transitions between them
2. **Cooldown Period**: The cooldown after selling is handled by not allowing a buy immediately after a sell
3. **Initialization**: Proper initialization of the first day's states is crucial
4. **Final State**: The answer is the maximum of the sold or rest states on the last day

## Edge Cases
1. Empty array or single day (return 0)
2. Continuously decreasing prices (no profit possible)
3. Continuously increasing prices (buy first, sell last)
4. All prices are the same (no profit)
5. Large input size (tests efficiency)

## Follow-up
How would you modify the solution if there was a transaction fee for each buy/sell operation?