# 704. Binary Search
**Easy**

## Problem Statement
Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return -1.

You must write an algorithm with O(log n) runtime complexity.

## Examples
### Example 1:
- Input: nums = [-1,0,3,5,9,12], target = 9
- Output: 4
- Explanation: 9 exists in nums and its index is 4

### Example 2:
- Input: nums = [-1,0,3,5,9,12], target = 2
- Output: -1
- Explanation: 2 does not exist in nums so return -1

## Constraints
- 1 <= nums.length <= 10^4
- -10^4 < nums[i], target < 10^4
- All the integers in `nums` are unique.
- `nums` is sorted in ascending order.

## Solution
```java
class Solution {
	public int search(int[] nums, int target) {
		int left=0,right=nums.length-1;
		for(int i=0;i<nums.length;i++){
			int mid=(right+left)/2;
			if(nums[mid]==target) {
				return mid;
			}
			else if(nums[mid]<target){
				left++;
			}else{
				right--;
			}
		}
		return -1;
	}
}
```

Approach

- Use two pointers, `left` and `right`, to represent the current search bounds.
- Calculate the middle index and compare the value to the target.
- If the middle value equals the target, return the index.
- If the middle value is less than the target, move the left pointer up.
- If the middle value is greater than the target, move the right pointer down.
- Repeat until the target is found or the search bounds cross.

Time Complexity

O(log n), where n is the length of `nums`.

Space Complexity

O(1), as the search is performed in-place.
