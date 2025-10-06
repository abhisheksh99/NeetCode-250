# 162. Find Peak Element

## Problem Statement
A peak element is an element that is strictly greater than its neighbors.

Given a 0-indexed integer array `nums`, find a peak element, and return its index. If the array contains multiple peaks, return the index to any of the peaks.

You may imagine that `nums[-1] = nums[n] = -∞`. In other words, an element is always considered to be strictly greater than a neighbor that is outside the array.

You must write an algorithm that runs in `O(log n)` time.

### Examples

#### Example 1:
```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```

#### Example 2:
```
Input: nums = [1,2,1,3,5,6,4]
Output: 5
Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.
```

## Approach
This problem can be efficiently solved using a binary search approach to achieve O(log n) time complexity. The key insight is that if an element is not a peak, then there must be a peak in the direction of the higher neighbor.

1. **Binary Search**:
   - Initialize two pointers, `left` and `right`, to the start and end of the array, respectively.
   - While `left < right`, calculate the middle index `mid`.
   - Compare the element at `mid` with the element at `mid + 1`:
     - If `nums[mid] < nums[mid + 1]`, the peak is in the right half, so set `left = mid + 1`.
     - Otherwise, the peak is in the left half, so set `right = mid`.
   - When the loop ends, `left` will be pointing to a peak element.

### Solution Code
```java
class Solution {
    public int findPeakElement(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            // Check if mid is a peak
            if (nums[mid] < nums[mid + 1]) {
                // If the next element is greater, search in the right half
                left = mid + 1;
            } else {
                // Otherwise, search in the left half
                right = mid;
            }
        }
        
        // When left == right, we've found a peak
        return left;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(log n), where n is the length of the array. The binary search reduces the search space by half in each iteration.
- **Space Complexity**: O(1), as we use a constant amount of extra space.

## Key Insights
1. **Binary Search**: The problem can be reduced to a binary search problem by leveraging the property that if an element is not a peak, then there must be a peak in the direction of the higher neighbor.
2. **Edge Handling**: The problem guarantees that `nums[-1] = nums[n] = -∞`, which means the array always has at least one peak.
3. **Multiple Peaks**: The problem allows returning any peak, which simplifies the solution as we don't need to find all peaks.

## Edge Cases
1. **Single Element**: If the array has only one element, it is the peak.
2. **Increasing Order**: If the array is strictly increasing, the last element is a peak.
3. **Decreasing Order**: If the array is strictly decreasing, the first element is a peak.
4. **All Elements Equal**: If all elements are the same, any index is a valid peak.

## Follow-up
How would you modify the solution to find all peak elements in the array? (This would require a linear scan to identify all local maxima.)