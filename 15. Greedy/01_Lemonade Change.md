# 860. Lemonade Change
**Easy**

## Problem Statement
At a lemonade stand, each lemonade costs $5. Customers are standing in a queue to buy from you and order one lemonade each. Each customer will only buy one lemonade and pay with either a $5, $10, or $20 bill. You must provide the correct change to each customer so that the net transaction is that the customer pays $5.

Return `true` if you can provide every customer with the correct change, or `false` otherwise.

## Examples
### Example 1:
- Input: `bills = [5,5,5,10,20]`
- Output: `true`
- Explanation: 
  - From the first 3 customers, we collect three $5 bills in order.
  - The fourth customer pays with a $10 bill and we return a $5.
  - The fifth customer pays with a $20 bill and we return a $10 and a $5.
  - Since all customers received correct change, we output true.

### Example 2:
- Input: `bills = [5,5,10,10,20]`
- Output: `false`
- Explanation: 
  - From the first two customers, we collect two $5 bills.
  - For the next two customers, we collect a $10 bill and give back a $5 each.
  - For the last customer, we cannot give change of $15 because we only have two $10 bills.
  - Since not every customer received correct change, the answer is false.
## Constraints
- `1 <= bills.length <= 10^5`
- `bills[i]` is either 5, 10, or 20.

## Solution
```java
class Solution {
    public boolean lemonadeChange(int[] bills) {
        int five = 0, ten = 0;
        for (int bill : bills) {
            if (bill == 5) {
                five++;
            } else if (bill == 10) {
                if (five == 0) return false;
                five--;
                ten++;
            } else { // bill == 20
                if (ten > 0 && five > 0) {
                    ten--;
                    five--;
                } else if (five >= 3) {
                    five -= 3;
                } else {
                    return false;
                }
            }
        }
        return true;
    }
}
```
## Approach
1. **Greedy Algorithm**: The solution uses a greedy approach to always use the largest denomination first when giving change to minimize the number of bills used.
2. **Bill Tracking**: We maintain counters for $5 and $10 bills (we don't need to track $20 bills as they can't be used for change).
3. **Change Calculation**:
   - For a $5 bill: Simply take it (no change needed).
   - For a $10 bill: Give back a $5 bill if available.
   - For a $20 bill: Try to give one $10 and one $5 first (optimal), or three $5 bills if $10 bills are not available.

## Time Complexity
- **O(n)**: We process each bill exactly once, where n is the number of bills.

## Space Complexity
- **O(1)**: We only use a constant amount of extra space (two integer variables) regardless of the input size.

## Edge Cases
- All customers pay with $5 bills.
- Customers alternate between different bill denominations.
- Large number of customers (up to 10^5 as per constraints).
- Cases where we need to give change using three $5 bills for a $20 bill.
