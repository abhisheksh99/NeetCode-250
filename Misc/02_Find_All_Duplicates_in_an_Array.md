# 442. Find All Duplicates in an Array

## Problem Statement
Given an integer array `nums` of length `n` where all the integers of `nums` are in the range `[1, n]` and each integer appears **once** or **twice**, return an array of all the integers that appear **twice**.

## Examples

### Example 1:
```
Input: nums = [4,3,2,7,8,2,3,1]
Output: [2,3]
```

### Example 2:
```
Input: nums = [1,1,2]
Output: [1]
```

### Example 3:
```
Input: nums = [1]
Output: []
```

## Approach
We can solve this problem in O(n) time with O(1) extra space by using the input array itself as a hash table. The key insight is to use the array indices to mark the presence of numbers by making the value at their corresponding index negative. If we encounter a negative value at an index, it means we've seen the number before, so we add it to our result.

## Solution Code
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        List<Integer> result = new ArrayList<>();
        
        for (int i = 0; i < nums.length; i++) {
            // Get the absolute value to handle negative numbers
            int index = Math.abs(nums[i]) - 1;  // Convert to 0-based index
            
            // If the value at index is negative, it means we've seen this number before
            if (nums[index] < 0) {
                result.add(index + 1);  // Add the number (1-based) to result
            } else {
                // Mark the number as seen by making the value at index negative
                nums[index] = -nums[index];
            }
        }
        
        return result;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n) - We make a single pass through the array
- **Space Complexity**: O(1) - We only use a constant amount of extra space (the result list is not counted as extra space as per problem constraints)

## Key Insights
1. **In-place Marking**: We use the input array to mark the presence of numbers by negating values at their corresponding indices.
2. **Index Mapping**: We map each number to its 0-based index by subtracting 1, since the numbers are in the range [1, n].
3. **Duplicate Detection**: If we encounter a negative value at an index, it means we've seen the number before, so we add it to our result.
4. **No Extra Space**: The solution doesn't use any additional data structures to track seen numbers, making it very space-efficient.

## Edge Cases
1. Empty array (should return empty list)
2. Array with no duplicates (should return empty list)
3. Array where all numbers are the same (should return that number)
4. Large input size (tests efficiency)

## Follow-up
How would you modify the solution if you need to return the duplicates in the order they first appear in the original array?