# 35. Search Insert Position
**Easy**

## Problem Statement
Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with O(log n) runtime complexity.

## Examples
### Example 1:
- Input: nums = [1,3,5,6], target = 5
- Output: 2

### Example 2:
- Input: nums = [1,3,5,6], target = 2
- Output: 1

### Example 3:
- Input: nums = [1,3,5,6], target = 7
- Output: 4

## Constraints
- 1 <= nums.length <= 10^4
- -10^4 <= nums[i], target <= 10^4
- nums contains distinct values sorted in ascending order.

## Solution
```java
public class Solution {
	public int searchInsert(int[] nums, int target) {
		int l = 0, r = nums.length - 1;
		while (l <= r) {
			int mid = (l + r) / 2;
			if (nums[mid] == target) {
				return mid;
			}
			if (nums[mid] > target) {
				r = mid - 1;
			} else {
				l = mid + 1;
			}
		}
		return l;
	}
}
```

Approach

- Use binary search to find the target in the sorted array.
- If the target is found, return its index.
- If not found, return the left pointer, which indicates the correct insert position.

Time Complexity

O(log n), where n is the length of `nums`.

Space Complexity

O(1), as the search is performed in-place.
