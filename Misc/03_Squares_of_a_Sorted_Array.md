# 977. Squares of a Sorted Array

## Problem Statement
Given an integer array `nums` sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

## Examples

### Example 1:
```
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
```

### Example 2:
```
Input: nums = [-7,-3,2,3,11]
Output: [4,9,9,49,121]
```

## Approach
We can solve this problem in O(n) time using a two-pointer approach. The key insight is that the array is already sorted, so the squares of the numbers will be largest at the ends of the array (due to negative numbers). We can use two pointers starting from the beginning and end of the array, compare their squares, and place the larger square at the end of the result array.

## Solution Code
```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        int left = 0;
        int right = n - 1;
        
        // Fill the result array from the end
        for (int i = n - 1; i >= 0; i--) {
            int square;
            // Compare absolute values to handle negative numbers
            if (Math.abs(nums[left]) < Math.abs(nums[right])) {
                square = nums[right] * nums[right];
                right--;
            } else {
                square = nums[left] * nums[left];
                left++;
            }
            result[i] = square;
        }
        
        return result;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n) - We make a single pass through the array
- **Space Complexity**: O(n) - We use an additional array to store the result

## Key Insights
1. **Two-pointer Technique**: Using two pointers allows us to process the array in a single pass.
2. **Largest Squares at Ends**: The largest squares will come from either the most negative or most positive numbers, which are at the ends of the array.
3. **In-place Comparison**: By comparing absolute values, we can determine which square is larger without actually squaring both numbers.
4. **Filling from End**: We fill the result array from the end to ensure the largest squares are placed correctly.

## Edge Cases
1. Array with all positive numbers
2. Array with all negative numbers
3. Array with a single element
4. Array with alternating positive and negative numbers
5. Large input size (tests efficiency)

## Follow-up
How would you modify the solution if you needed to sort the squares in descending order instead of ascending order?