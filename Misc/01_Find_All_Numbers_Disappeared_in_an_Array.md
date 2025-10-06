# 448. Find All Numbers Disappeared in an Array

## Problem Statement
Given an array `nums` of `n` integers where `nums[i]` is in the range `[1, n]`, return an array of all the integers in the range `[1, n]` that do not appear in `nums`.

## Examples

### Example 1:
```
Input: nums = [4,3,2,7,8,2,3,1]
Output: [5,6]
```

### Example 2:
```
Input: nums = [1,1]
Output: [2]
```

## Approach
We can solve this problem in O(n) time with O(1) extra space by using the input array itself as a hash table. The idea is to mark the presence of numbers by negating the value at their corresponding index. After processing all numbers, the indices with positive values will indicate the missing numbers.

## Solution Code
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> result = new ArrayList<>();
        
        // Mark the presence of each number by negating the value at its corresponding index
        for (int i = 0; i < nums.length; i++) {
            int index = Math.abs(nums[i]) - 1;  // Convert to 0-based index
            if (nums[index] > 0) {
                nums[index] = -nums[index];  // Mark as present by negating
            }
        }
        
        // Find all indices with positive values (1-based)
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) {
                result.add(i + 1);  // Convert back to 1-based
            }
        }
        
        return result;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n) - We make two passes through the array
- **Space Complexity**: O(1) - We only use a constant amount of extra space (the result list is not counted as extra space as per problem constraints)

## Key Insights
1. **In-place Marking**: We use the input array itself to mark the presence of numbers by negating values at their corresponding indices.
2. **Index Mapping**: We map each number to its 0-based index by subtracting 1, since the numbers are in the range [1, n].
3. **No Extra Space**: The solution doesn't use any additional data structures to track seen numbers, making it very space-efficient.

## Edge Cases
1. Empty array (should return empty list)
2. Array with all numbers present (should return empty list)
3. Array with all numbers the same (should return all other numbers in range)
4. Large input size (tests efficiency)

## Follow-up
How would you modify the solution if you need to return the missing numbers in sorted order without using extra space for sorting?