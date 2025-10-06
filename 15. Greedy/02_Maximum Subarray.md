# 53. Maximum Subarray
**Medium**

## Problem Statement
Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

## Examples
### Example 1:
- Input: `nums = [-2,1,-3,4,-1,2,1,-5,4]`
- Output: `6`
- Explanation: The subarray `[4,-1,2,1]` has the largest sum 6.

### Example 2:
- Input: `nums = [1]`
- Output: `1`
- Explanation: The subarray `[1]` has the largest sum 1.

### Example 3:
- Input: `nums = [5,4,-1,7,8]`
- Output: `23`
- Explanation: The subarray `[5,4,-1,7,8]` has the largest sum 23.

## Constraints
- `1 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`

## Solution
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int currSum = 0;
        int maxSub = nums[0];
        
        for (int num : nums) {
            if (currSum < 0) {
                currSum = 0;
            }
            currSum += num;
            maxSub = Math.max(maxSub, currSum);
        }
        return maxSub;
    }
}
```
## Approach
1. **Kadane's Algorithm**: This solution uses Kadane's algorithm to find the maximum subarray sum in O(n) time.
2. **Tracking Current and Maximum Sum**:
   - `currSum` keeps track of the sum of the current subarray.
   - If `currSum` becomes negative, we reset it to 0 (since a negative sum would only decrease the sum of a future subarray).
   - `maxSub` keeps track of the maximum sum encountered so far.
3. **Single Pass**: The algorithm processes each element exactly once, making it efficient.

## Time Complexity
- **O(n)**: We traverse the array once, where n is the length of the input array.

## Space Complexity
- **O(1)**: We use a constant amount of extra space, regardless of the input size.

## Edge Cases
- All negative numbers: The maximum subarray would be the largest single element.
- All positive numbers: The entire array is the maximum subarray.
- A single element: The single element is the maximum subarray.
- Large input size: The solution efficiently handles the upper constraint of 10^5 elements.