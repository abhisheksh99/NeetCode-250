# 904. Fruit Into Baskets

## Problem Statement
You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array `fruits` where `fruits[i]` is the type of fruit the `i-th` tree produces.

You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:

1. You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.
2. Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.
3. Once you reach a tree with fruit that cannot fit in your baskets, you must stop.

Given the integer array `fruits`, return the maximum number of fruits you can pick.

## Examples

### Example 1:
```
Input: fruits = [1,2,1]
Output: 3
Explanation: We can pick from all 3 trees.
```

### Example 2:
```
Input: fruits = [0,1,2,2]
Output: 3
Explanation: We can pick from trees [1,2,2].
If we had started at the first tree, we would only pick from trees [0,1].
```

### Example 3:
```
Input: fruits = [1,2,3,2,2]
Output: 4
Explanation: We can pick from trees [2,3,2,2].
If we had started at the first tree, we would only pick from trees [1,2].
```

## Approach
This problem is a variation of the classic "longest substring with at most two distinct characters" problem. We can solve it using a sliding window approach with a hash map to keep track of the count of each fruit type in the current window.

1. **Sliding Window with Hash Map**:
   - Use a hash map to store the count of each fruit type in the current window.
   - Maintain two pointers, `left` and `right`, representing the current window.
   - Expand the window by moving `right` and add the fruit to the map.
   - If the map size exceeds 2, shrink the window from the left until the map size is back to 2.
   - Update the maximum length of the window during the process.

2. **Optimization**:
   - Instead of using a hash map, we can use an array of fixed size (since the number of fruit types is limited) for better performance.
   - We can also keep track of the last occurrence of each fruit type to optimize the window shrinking step.

## Solution Code
```java
class Solution {
    public int totalFruit(int[] fruits) {
        // This is a problem of finding the longest subarray with at most 2 distinct elements
        // We'll use a sliding window approach with a hash map to keep track of counts
        
        int maxFruits = 0;
        int left = 0;
        Map<Integer, Integer> fruitCount = new HashMap<>();
        
        for (int right = 0; right < fruits.length; right++) {
            // Add current fruit to the basket
            int currentFruit = fruits[right];
            fruitCount.put(currentFruit, fruitCount.getOrDefault(currentFruit, 0) + 1);
            
            // If we have more than 2 types of fruits, shrink the window from the left
            while (fruitCount.size() > 2) {
                int leftFruit = fruits[left];
                fruitCount.put(leftFruit, fruitCount.get(leftFruit) - 1);
                if (fruitCount.get(leftFruit) == 0) {
                    fruitCount.remove(leftFruit);
                }
                left++;
            }
            
            // Update the maximum number of fruits
            maxFruits = Math.max(maxFruits, right - left + 1);
        }
        
        return maxFruits;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n), where n is the number of fruits. Each fruit is processed exactly once.
- **Space Complexity**: O(1), since the hash map can have at most 3 entries (since we're only allowing 2 types of fruits at a time).

## Key Insights
1. **Sliding Window**: This problem is a classic sliding window problem where we need to find the longest subarray with at most two distinct elements.
2. **Hash Map for Counting**: We use a hash map to keep track of the count of each fruit type in the current window.
3. **Window Shrinking**: When we encounter a third type of fruit, we shrink the window from the left until we only have two types of fruits again.

## Edge Cases
1. **Single Type of Fruit**: The array contains only one type of fruit (e.g., `[1,1,1,1]`).
2. **Two Types of Fruits**: The array contains exactly two types of fruits (e.g., `[1,2,1,2,1]`).
3. **All Fruits are the Same**: The array contains only one type of fruit (e.g., `[0,0,0,0]`).
4. **Large Input**: The array contains the maximum number of fruits (10^5) with alternating types.

## Follow-up
How would you modify the solution if you were allowed to have `k` baskets instead of 2? (This would involve changing the condition in the while loop to check if the number of distinct fruits exceeds `k`.)