# 16. 3Sum Closest

## Problem Statement
Given an integer array `nums` of length `n` and an integer `target`, find three integers in `nums` such that the sum is closest to `target`. Return the sum of the three integers.

You may assume that each input would have exactly one solution.

### Examples

#### Example 1:
```
Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

#### Example 2:
```
Input: nums = [0,0,0], target = 1
Output: 0
Explanation: The sum that is closest to the target is 0. (0 + 0 + 0 = 0).
```

## Approach
This problem can be solved using a two-pointer approach after sorting the array. The key idea is to iterate through the array and for each element, use two pointers to find the other two elements whose sum with the current element is closest to the target.

1. **Sort the Array**: Sorting helps in efficiently finding the closest sum using two pointers.
2. **Initialize Variables**: Keep track of the closest sum found so far.
3. **Iterate Through the Array**: For each element at index `i`, use two pointers (`left` and `right`) to find the other two elements.
4. **Update Closest Sum**: For each triplet, calculate the sum and update the closest sum if the current sum is closer to the target.
5. **Adjust Pointers**: Move the pointers based on whether the current sum is less than or greater than the target.

### Solution Code
```java
import java.util.Arrays;

class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int closestSum = nums[0] + nums[1] + nums[2];
        
        for (int i = 0; i < nums.length - 2; i++) {
            int left = i + 1;
            int right = nums.length - 1;
            
            while (left < right) {
                int currentSum = nums[i] + nums[left] + nums[right];
                
                // Update closestSum if currentSum is closer to target
                if (Math.abs(currentSum - target) < Math.abs(closestSum - target)) {
                    closestSum = currentSum;
                }
                
                // Adjust pointers
                if (currentSum < target) {
                    left++;
                } else if (currentSum > target) {
                    right--;
                } else {
                    // If we find a sum equal to target, return it immediately
                    return currentSum;
                }
            }
        }
        
        return closestSum;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n²), where n is the length of the array. The outer loop runs in O(n) time, and the inner two-pointer approach runs in O(n) time for each iteration of the outer loop.
- **Space Complexity**: O(log n) to O(n) depending on the sorting algorithm used. The space complexity is dominated by the sorting step.

## Key Insights
1. **Sorting**: Sorting the array allows us to use the two-pointer technique to efficiently find the closest sum.
2. **Two-Pointer Technique**: By fixing one element and using two pointers to find the other two elements, we can reduce the problem to a linear scan for each fixed element.
3. **Early Termination**: If we find a sum equal to the target, we can immediately return it as it is the closest possible sum.

## Edge Cases
1. **Small Array**: If the array has exactly three elements, return their sum.
2. **All Elements Equal**: If all elements are the same, the sum will be three times any element.
3. **Large Target**: If the target is very large, the sum of the three largest elements will be the closest.
4. **Negative Numbers**: The array can contain negative numbers, and the target can also be negative.

## Follow-up
How would you modify the solution to find the k-sum closest to the target, where k is a given integer? (This would involve a more general approach using recursion or dynamic programming.)