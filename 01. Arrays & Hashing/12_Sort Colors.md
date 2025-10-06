# 75. Sort Colors
**Medium**

## Problem Statement
Given an array `nums` with n objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.
We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.
You must solve this problem without using the library's sort function.

## Examples
### Example 1:
- Input: nums = [2,0,2,1,1,0]
- Output: [0,0,1,1,2,2]

### Example 2:
- Input: nums = [2,0,1]
- Output: [0,1,2]

## Constraints
- 1 <= nums.length <= 300
- nums[i] is 0, 1, or 2.

## Solution
```java
class Solution {
	public void sortColors(int[] nums) {
		if (nums.length == 0 || nums.length == 1) return;

		int start = 0, end = nums.length - 1;
		int index = 0;

		while (index <= end) {
			if (nums[index] == 0) {
				swap(nums, index, start);
				start++;
				index++;
			} else if (nums[index] == 2) {
				swap(nums, index, end);
				end--;
			} else {
				index++;
			}
		}
	}

	private void swap(int[] nums, int i, int j) {
		int temp = nums[i];
		nums[i] = nums[j];
		nums[j] = temp;
	}
}
```

Approach

- Use the Dutch National Flag algorithm with three pointers: `start`, `end`, and `index`.
- Initialize `start` at 0, `end` at the last index, and `index` at 0.
- Iterate through the array:
  - If `nums[index]` is 0, swap it with `nums[start]`, increment both `start` and `index`.
  - If `nums[index]` is 2, swap it with `nums[end]`, decrement `end` (do not increment `index`).
  - If `nums[index]` is 1, just increment `index`.
- Continue until `index` passes `end`.
- Notes: This sorts the array in a single pass and in-place.

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(1), as sorting is done in-place.
