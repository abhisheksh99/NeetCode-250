# 135. Candy

## Problem Statement
There are `n` children standing in a line. Each child is assigned a rating value given in the integer array `ratings`.

You are giving candies to these children subjected to the following requirements:
- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

Return the minimum number of candies you need to have to distribute the candies to the children.

### Examples

**Example 1:**
```
Input: ratings = [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.
```

**Example 2:**
```
Input: ratings = [1,2,2]
Output: 4
Explanation: You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
The third child gets 1 candy because it satisfies the above two conditions.
```

### Constraints:
- `n == ratings.length`
- `1 <= n <= 2 * 10^4`
- `0 <= ratings[i] <= 2 * 10^4`

## Solution
```java
import java.util.Arrays;

class Solution {
    public int candy(int[] ratings) {
        int n = ratings.length;
        int[] candies = new int[n];
        Arrays.fill(candies, 1);

        // Left to right: if rating[i] > rating[i - 1], increment candy count
        for (int i = 1; i < n; i++) {
            if (ratings[i] > ratings[i - 1]) {
                candies[i] = candies[i - 1] + 1;
            }
        }

        // Right to left: if rating[i] > rating[i + 1], update candy count to max needed
        for (int i = n - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1]) {
                candies[i] = Math.max(candies[i], candies[i + 1] + 1);
            }
        }

        int totalCandies = 0;
        for (int candy : candies) {
            totalCandies += candy;
        }
        return totalCandies;
    }
}
```

## Approach
1. **Initialize Candies**: Start by giving each child exactly one candy.
2. **Left to Right Pass**: Traverse the ratings array from left to right. For each child, if their rating is higher than the previous child, give them one more candy than the previous child.
3. **Right to Left Pass**: Traverse the ratings array from right to left. For each child, if their rating is higher than the next child, ensure they have at least one more candy than the next child (using `Math.max` to preserve any higher value from the left pass).
4. **Calculate Total**: Sum up all the candies to get the minimum number of candies needed.

## Complexity
- **Time Complexity**: O(n), where n is the number of children. We make three passes through the array: one for initialization, one left-to-right pass, and one right-to-left pass.
- **Space Complexity**: O(n), as we use an additional array of size n to store the candies for each child.

## Edge Cases
- A single child should receive exactly 1 candy.
- All children have the same rating, so each gets exactly 1 candy.
- Strictly increasing or decreasing ratings.
- Alternating high and low ratings.
- The maximum constraint case with 20,000 children should be handled efficiently.
- All children have the minimum (0) or maximum (20,000) possible rating.