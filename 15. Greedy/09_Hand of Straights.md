# 846. Hand of Straights
**Medium**

## Problem Statement
Alice has some number of cards and she wants to rearrange the cards into groups so that each group is of size `groupSize`, and consists of `groupSize` consecutive cards.

Given an integer array `hand` where `hand[i]` is the value written on the `i-th` card and an integer `groupSize`, return `true` if she can rearrange the cards, or `false` otherwise.

## Examples
### Example 1:
- Input: `hand = [1,2,3,6,2,3,4,7,8]`, `groupSize = 3`
- Output: `true`
- Explanation: Alice's hand can be rearranged as `[1,2,3],[2,3,4],[6,7,8]`

### Example 2:
- Input: `hand = [1,2,3,4,5]`, `groupSize = 4`
- Output: `false`
- Explanation: Alice's hand cannot be rearranged into groups of 4.

## Constraints
- `1 <= hand.length <= 10^4`
- `0 <= hand[i] <= 10^9`
- `1 <= groupSize <= hand.length`

## Solution
```java
import java.util.TreeMap;

class Solution {
    public boolean isNStraightHand(int[] hand, int groupSize) {
        // Early exit if total cards can't be divided into groups of groupSize
        if (hand.length % groupSize != 0) return false;

        // Use TreeMap to store card counts in sorted order
        TreeMap<Integer, Integer> cardCounts = new TreeMap<>();
        
        // Count each card
        for (int card : hand) {
            cardCounts.put(card, cardCounts.getOrDefault(card, 0) + 1);
        }
        
        // Attempt to form groups
        while (!cardCounts.isEmpty()) {
            // Start with the smallest available card
            int first = cardCounts.firstKey();
            
            // Try to form a group starting from 'first'
            for (int i = 0; i < groupSize; i++) {
                int currentCard = first + i;
                
                // If we don't have the current card, can't form the group
                if (!cardCounts.containsKey(currentCard)) {
                    return false;
                }
                
                // Decrease the count of the current card
                int count = cardCounts.get(currentCard);
                if (count == 1) {
                    cardCounts.remove(currentCard);
                } else {
                    cardCounts.put(currentCard, count - 1);
                }
            }
        }
        
        return true;
    }
}
```

## Approach
1. **Greedy Algorithm with TreeMap**:
   - First, check if the total number of cards is divisible by `groupSize` (early exit if not).
   - Use a `TreeMap` to keep track of card counts in sorted order.
   - In each iteration, start with the smallest available card and try to form a group of consecutive cards.
   - For each card in the group, decrement its count in the map, removing it if the count reaches zero.
   - If at any point a required consecutive card is missing, return false.

2. **Key Insights**:
   - The greedy approach of always starting with the smallest available card ensures we don't miss any potential groups.
   - Using `TreeMap` gives us O(log n) access to the smallest card and maintains the cards in sorted order.
   - The algorithm efficiently handles the grouping by processing one complete group at a time.

## Time Complexity
- **O(n log n)**: The TreeMap operations (put, get, remove) take O(log n) time, and we perform these operations for each card.
- **O(n)**: In the best case where all cards are the same and groupSize is 1, but generally O(n log n) dominates.

## Space Complexity
- **O(n)**: We store each card in the TreeMap, where n is the number of unique cards.

## Edge Cases
- Single group where groupSize equals the number of cards
- All cards are the same value
- Large input size (10^4 cards)
- Large card values (up to 10^9)
- groupSize = 1 (always returns true if hand is not empty)