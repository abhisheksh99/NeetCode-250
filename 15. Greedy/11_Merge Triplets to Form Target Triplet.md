# 1899. Merge Triplets to Form Target Triplet
**Medium**

## Problem Statement
A **triplet** is an array of three integers. You are given a 2D integer array `triplets`, where `triplets[i] = [ai, bi, ci]` describes the `i-th` **triplet**. You are also given an integer array `target = [x, y, z]` that describes the **target triplet**.

In one operation, you can choose any two triplets from `triplets` and create a new triplet by taking the **maximum** of their corresponding elements. For example, `[1, 5, 7]` and `[3, 2, 1]` can be merged to form `[max(1, 3), max(5, 2), max(7, 1)] = [3, 5, 7]`.

Return `true` if it is possible to obtain the target triplet `[x, y, z]` as an element-wise maximum of some pair of triplets in `triplets`, or `false` otherwise.

## Examples
### Example 1:
- Input: `triplets = [[2,5,3],[1,8,4],[1,7,5]]`, `target = [2,7,5]`
- Output: `true`
- Explanation: 
  - Take the first and last triplets.
  - [2,5,3] and [1,7,5] → [max(2,1), max(5,7), max(3,5)] = [2,7,5] = target

### Example 2:
- Input: `triplets = [[1,3,4],[2,5,8]]`, `target = [2,5,8]`
- Output: `true`
- Explanation: The target triplet [2,5,8] already exists in triplets.

### Example 3:
- Input: `triplets = [[2,5,3],[2,3,4],[1,2,5],[5,2,3]]`, `target = [5,5,5]`
- Output: `true`
- Explanation: 
  - Take the first and third triplets.
  - [2,5,3] and [1,2,5] → [max(2,1), max(5,2), max(3,5)] = [2,5,5]
  - Then take this result with the fourth triplet.
  - [2,5,5] and [5,2,3] → [max(2,5), max(5,2), max(5,3)] = [5,5,5]

## Constraints
- `1 <= triplets.length <= 10^5`
- `triplets[i].length == 3`
- `target.length == 3`
- `0 <= ai, bi, ci, x, y, z <= 1000`

## Solution
```java
class Solution {
    public boolean mergeTriplets(int[][] triplets, int[] target) {
        // Initialize the max values for each element in the triplet
        int[] maxValues = new int[3];

        // Iterate over each triplet
        for (int[] triplet : triplets) {
            // Check if the current triplet can contribute to the target
            if (triplet[0] <= target[0] && triplet[1] <= target[1] && triplet[2] <= target[2]) {
                // Update the max values for each element
                maxValues[0] = Math.max(maxValues[0], triplet[0]);
                maxValues[1] = Math.max(maxValues[1], triplet[1]);
                maxValues[2] = Math.max(maxValues[2], triplet[2]);
            }
        }

        // Check if the max values match the target triplet
        return maxValues[0] == target[0] && maxValues[1] == target[1] && maxValues[2] == target[2];
    }
}
```

## Approach
1. **Greedy Maximum Tracking**:
   - Initialize an array `maxValues` to keep track of the maximum values for each position in the triplet.
   - For each triplet, check if all its elements are less than or equal to the corresponding elements in the target. This ensures the triplet can be part of the solution.
   - If a triplet is valid, update the `maxValues` by taking the element-wise maximum with the current values.
   - Finally, check if the `maxValues` match the target exactly.

2. **Key Insights**:
   - Only triplets where all elements are ≤ the corresponding target elements can contribute to the solution.
   - The maximum values across all valid triplets will form the target if it's possible to do so.
   - If any element in `maxValues` doesn't match the target, it means we couldn't achieve that particular value in the target.

## Time Complexity
- **O(n)**: We make a single pass through the array of triplets, where n is the number of triplets.
- Each operation inside the loop is O(1).

## Space Complexity
- **O(1)**: We use a constant amount of extra space (just an array of size 3).

## Edge Cases
- Single triplet that matches the target
- No triplets can form the target
- All triplets are the same
- Large input size (10^5 triplets)
- Target has maximum possible values (1000, 1000, 1000)
- All triplets have the same value for one or more positions