# 713. Subarray Product Less Than K

## Problem Statement
Given an array of integers `nums` and an integer `k`, return the number of contiguous subarrays where the product of all the elements in the subarray is strictly less than `k`.

### Examples

#### Example 1:
```
Input: nums = [10,5,2,6], k = 100
Output: 8
Explanation: The 8 subarrays that have product less than 100 are:
[10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6]
Note that [10, 5, 2] is not included as the product is 100 which is not strictly less than k.
```

#### Example 2:
```
Input: nums = [1,2,3], k = 0
Output: 0
```

## Approach
This problem can be efficiently solved using a sliding window approach to count the number of valid subarrays where the product is less than `k`.

1. **Sliding Window Technique**:
   - Use two pointers, `left` and `right`, to represent the current window.
   - Maintain a running product of the elements in the current window.
   - If the product is less than `k`, all subarrays ending at `right` and starting from `left` to `right` are valid.
   - If the product becomes greater than or equal to `k`, move the `left` pointer to the right until the product is less than `k` again.

2. **Key Insight**:
   - For each valid window `[left, right]`, the number of new subarrays ending at `right` is `right - left + 1`.
   - This accounts for all possible subarrays ending at `right` that satisfy the product condition.

### Solution Code
```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        if (k <= 1) return 0; // Since all numbers are positive, product cannot be less than 1
        
        int count = 0;
        int product = 1;
        int left = 0;
        
        for (int right = 0; right < nums.length; right++) {
            product *= nums[right];
            
            // Shrink the window from the left until product is less than k
            while (product >= k) {
                product /= nums[left];
                left++;
            }
            
            // All subarrays ending at 'right' with start from 'left' to 'right' are valid
            count += right - left + 1;
        }
        
        return count;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n), where n is the length of `nums`. Each element is processed exactly twice (once when expanding the window and once when shrinking).
- **Space Complexity**: O(1), as we use a constant amount of extra space.

## Key Insights
1. **Sliding Window**: The sliding window approach efficiently tracks the product of the current window and adjusts the window size to maintain the product less than `k`.
2. **Subarray Counting**: For each valid window, the number of new subarrays ending at the current right pointer is calculated in constant time.
3. **Early Termination**: If `k` is 0 or 1, we can immediately return 0 since all numbers are positive and their product cannot be less than 1.

## Edge Cases
1. **Empty Array**: If the input array is empty, the result should be 0.
2. **Single Element**: If the array has only one element, check if it is less than `k`.
3. **All Elements Greater Than k**: If all elements are greater than or equal to `k`, the result should be 0.
4. **k = 0 or 1**: Since all numbers are positive, the product cannot be less than 1, so the result is 0.

## Follow-up
How would you modify the solution if the array could contain zeros and negative numbers?