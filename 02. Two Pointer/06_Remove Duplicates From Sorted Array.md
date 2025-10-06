# 26. Remove Duplicates from Sorted Array
**Easy**

## Problem Statement
Given an integer array `nums` sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Return the number of unique elements in `nums`.
Do not allocate extra space for another array; you must do this by modifying the input array in-place with O(1) extra memory.

## Examples
### Example 1:
- Input: nums = [1,1,2]
- Output: 2, nums = [1,2,_]

### Example 2:
- Input: nums = [0,0,1,1,1,2,2,3,3,4]
- Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]

## Constraints
- 1 <= nums.length <= 3 * 10^4
- -100 <= nums[i] <= 100
- nums is sorted in non-decreasing order.

## Solution
```java
class Solution {
	public int removeDuplicates(int[] nums) {
		if (nums.length == 0) return 0;

		int count = 1; // Start from 1, since the first element is always unique

		for (int i = 1; i < nums.length; i++) {
			if (nums[i] != nums[i - 1]) {
				nums[count] = nums[i];
				count++;
			}
		}

		return count;
	}
}
```

Approach

- Use a pointer `count` to track the position of the next unique element.
- Iterate through the array starting from the second element.
- If the current element is different from the previous, place it at the `count` index and increment `count`.
- Continue until the end of the array; the first `count` elements are unique.
- Notes: The array is modified in-place, and extra space is not used.

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(1), as the operation is done in-place.
