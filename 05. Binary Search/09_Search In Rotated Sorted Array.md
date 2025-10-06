# 33. Search in Rotated Sorted Array
**Medium**

## Problem Statement
There is an integer array `nums` sorted in ascending order (with distinct values), which is rotated at an unknown pivot index. Given the array and an integer `target`, return the index of `target` if it is in `nums`, or -1 if it is not in `nums`.

You must write an algorithm with O(log n) runtime complexity.

## Examples
### Example 1:
- Input: nums = [4,5,6,7,0,1,2], target = 0
- Output: 4

### Example 2:
- Input: nums = [4,5,6,7,0,1,2], target = 3
- Output: -1

### Example 3:
- Input: nums = [1], target = 0
- Output: -1

## Constraints
- 1 <= nums.length <= 5000
- -10^4 <= nums[i] <= 10^4
- All values of nums are unique.
- nums is guaranteed to be rotated at least once.

## Solution
```java
class Solution {
	public int search(int[] nums, int target) {
		int left = 0, right = nums.length - 1;

		while (left <= right) {
			int mid = left + (right - left) / 2;

			if (nums[mid] == target) return mid;

			// Left half is sorted
			if (nums[left] <= nums[mid]) {
				if (nums[left] <= target && target < nums[mid]) right = mid - 1;
				else left = mid + 1;
			} 
			// Right half is sorted
			else {
				if (nums[mid] < target && target <= nums[right]) left = mid + 1;
				else right = mid - 1;
			}
		}

		return -1;
	}
}
```

Approach

- Use binary search to find the target in a rotated sorted array.
- At each step, determine which half (left or right) is sorted.
- If the target is within the sorted half, adjust the search bounds accordingly.
- Otherwise, search in the other half.
- Repeat until the target is found or the search bounds cross.

Time Complexity

O(log n), where n is the length of `nums`.

Space Complexity

O(1), as the search is performed in-place.
