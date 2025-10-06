# 121. Best Time to Buy and Sell Stock
**Easy**

## Problem Statement
You are given an array `prices` where `prices[i]` is the price of a given stock on the i-th day.
You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.
Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

## Examples
### Example 1:
- Input: prices = [7,1,5,3,6,4]
- Output: 5
- Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.

### Example 2:
- Input: prices = [7,6,4,3,1]
- Output: 0
- Explanation: No transaction is done, i.e., max profit = 0.

## Constraints
- 1 <= prices.length <= 10^5
- 0 <= prices[i] <= 10^4

## Solution
```java
class Solution {
	public int maxProfit(int[] prices) {
		int left=0,right=1,maxP=0;
		while(right<prices.length){
			if(prices[right]>prices[left]){
				int profit=prices[right]-prices[left];
				maxP=Math.max(maxP,profit);
			}
			else{
				left=right;
			}
			right++;
		}
		return maxP;
	}
}
```

Approach

- Use two pointers: `left` for the buying day and `right` for the selling day.
- Move `right` through the array:
  - If `prices[right]` is greater than `prices[left]`, calculate the profit and update the maximum profit.
  - If not, move `left` to `right` (new minimum price).
- Continue until the end of the array.
- Notes: This approach finds the best buy/sell pair in a single pass.

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(1), as only a few pointers/variables are used.
