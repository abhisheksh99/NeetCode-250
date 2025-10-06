# 283. Move Zeroes

## Problem Statement
Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Note** that you must do this in-place without making a copy of the array.

## Examples

### Example 1:
```
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
```

### Example 2:
```
Input: nums = [0]
Output: [0]
```

## Approach
We can solve this problem in O(n) time with O(1) extra space using a two-pointer approach:
1. Use a pointer `lastNonZero` to keep track of the position where the next non-zero element should be placed.
2. Iterate through the array with another pointer `i`.
3. For each non-zero element, swap it with the element at `lastNonZero` and increment `lastNonZero`.
4. This approach ensures all non-zero elements are moved to the front while maintaining their relative order, and zeros are moved to the end.

## Solution Code
```java
class Solution {
    public void moveZeroes(int[] nums) {
        int lastNonZero = 0;
        
        // Move all non-zero elements to the front
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                // Swap non-zero element to its correct position
                int temp = nums[lastNonZero];
                nums[lastNonZero] = nums[i];
                nums[i] = temp;
                lastNonZero++;
            }
        }
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n) - We make a single pass through the array
- **Space Complexity**: O(1) - We only use a constant amount of extra space

## Key Insights
1. **In-place Operation**: The solution modifies the input array in-place without using extra space for another array.
2. **Two-pointer Technique**: We use two pointers - one to iterate through the array and another to track the position for the next non-zero element.
3. **Stable Sorting**: The relative order of non-zero elements is maintained, which is a key requirement.
4. **Efficient Swapping**: We only perform swaps when necessary, keeping the number of operations to a minimum.

## Edge Cases
1. Array with no zeros (should remain unchanged)
2. Array with all zeros (should remain unchanged)
3. Array with a single zero at the beginning/end
4. Array with alternating zeros and non-zeros
5. Large input size (tests efficiency)

## Follow-up
How would you modify the solution if you needed to move all zeros to the front while maintaining the relative order of non-zero elements?