# 152. Maximum Product Subarray

## Problem Statement
Given an integer array `nums`, find a contiguous non-empty subarray within the array that has the largest product, and return the product. The test cases are generated so that the answer will fit in a 32-bit integer.

A subarray is a contiguous subsequence of the array.

### Examples

**Example 1:**
```
Input: nums = [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```

**Example 2:**
```
Input: nums = [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

### Constraints:
- `1 <= nums.length <= 2 * 10^4`
- `-10 <= nums[i] <= 10`
- The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.

## Solution
```java
class Solution {
    public int maxProduct(int[] nums) {
        // Handle empty array case
        if (nums.length == 0) {
            return 0;
        }
        
        // Initialize variables to track maximum and minimum products
        int min = nums[0];
        int max = nums[0];
        int result = max;  // Final result to return
        
        for (int i = 1; i < nums.length; i++) {
            int current = nums[i];
            
            // Calculate new max and min products ending at current position
            // We need to consider three possibilities:
            // 1. Current number itself (start a new subarray)
            // 2. Product of current number and previous max (extend previous subarray)
            // 3. Product of current number and previous min (handles negative numbers)
            int temp = Math.max(current, Math.max(max * current, min * current));
            min = Math.min(current, Math.min(min * current, max * current));
            
            // Update max for next iteration
            max = temp;
            
            // Update the overall result
            result = Math.max(result, max);
        }
        
        return result;
    }
}
```

## Approach
1. **Dynamic Programming with State Tracking**:
   - We maintain three variables: `max`, `min`, and `result`.
   - `max` tracks the maximum product of subarrays ending at the current position.
   - `min` tracks the minimum product of subarrays ending at the current position (to handle negative numbers).
   - `result` keeps track of the overall maximum product found so far.

2. **Key Insight**:
   - The maximum product at each position can come from three possibilities:
     1. The current number itself (starting a new subarray).
     2. The product of the current number and the previous maximum product (extending the previous subarray).
     3. The product of the current number and the previous minimum product (handles the case where previous min is negative and current number is negative).

3. **Handling Negatives**:
   - We keep track of both maximum and minimum products because a negative number can turn a minimum product into a maximum product when multiplied by another negative number.

## Complexity
- **Time Complexity**: O(n)
  - We make a single pass through the array, performing constant time operations at each step.
- **Space Complexity**: O(1)
  - We use a constant amount of extra space (three variables) regardless of the input size.

## Edge Cases
- `nums = [0]` → `0` (single element array)
- `nums = [-2, -3, -4]` → `12` (all negative numbers)
- `nums = [0, 2]` → `2` (subarray with zero)
- `nums = [2, -5, -2, -4, 3]` → `24` (handles multiple negatives and zeros)

## Key Insights
- The main challenge is handling negative numbers, as two negatives can produce a positive product.
- By keeping track of both minimum and maximum products at each step, we can handle all possible cases where the product might flip from negative to positive or vice versa.
- The solution efficiently computes the maximum product subarray in a single pass through the array, making it optimal for large inputs.