# 81. Search in Rotated Sorted Array II
**Medium**

## Problem Statement
There is an integer array `nums` sorted in non-decreasing order (not necessarily with distinct values), which is rotated at an unknown pivot index. Given the array and an integer `target`, return true if `target` is in `nums`, or false if it is not in `nums`.

You must decrease the overall operation steps as much as possible.

## Examples
### Example 1:
- Input: nums = [2,5,6,0,0,1,2], target = 0
- Output: true

### Example 2:
- Input: nums = [2,5,6,0,0,1,2], target = 3
- Output: false

## Constraints
- 1 <= nums.length <= 5000
- -10^4 <= nums[i] <= 10^4
- nums is sorted in non-decreasing order.
- nums may contain duplicates.

## Solution
```java
class Solution {
	public boolean search(int[] nums, int target) {
		int left = 0, right = nums.length - 1;

		while (left <= right) {
			int mid = left + (right - left) / 2;
			if (nums[mid] == target) return true;
            
			// If duplicates at the boundaries, skip one from both sides
			if (nums[left] == nums[mid] && nums[mid] == nums[right]) {
				left++;
				right--;
			} 
			// Left half is sorted
			else if (nums[left] <= nums[mid]) {
				if (nums[left] <= target && target < nums[mid]) {
					right = mid - 1;  // Target in left half
				} else {
					left = mid + 1;   // Target in right half
				}
			} 
			// Right half is sorted
			else {
				if (nums[mid] < target && target <= nums[right]) {
					left = mid + 1;  // Target in right half
				} else {
					right = mid - 1; // Target in left half
				}
			}
		}

		return false;
	}
}
```

Approach

- Use binary search to find the target in a rotated sorted array with duplicates.
- If duplicates are at the boundaries (left, mid, right), skip one from both sides to reduce ambiguity.
- Determine which half is sorted and adjust the search bounds accordingly.
- Repeat until the target is found or the search bounds cross.

Time Complexity

O(n) in the worst case (when many duplicates are present), O(log n) on average.

Space Complexity

O(1), as the search is performed in-place.
