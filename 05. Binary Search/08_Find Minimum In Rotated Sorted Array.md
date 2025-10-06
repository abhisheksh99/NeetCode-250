# 153. Find Minimum in Rotated Sorted Array
**Medium**

## Problem Statement
Suppose an array of length n sorted in ascending order is rotated between 1 and n times. Given the rotated array `nums`, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.

## Examples
### Example 1:
- Input: nums = [3,4,5,1,2]
- Output: 1

### Example 2:
- Input: nums = [4,5,6,7,0,1,2]
- Output: 0

### Example 3:
- Input: nums = [11,13,15,17]
- Output: 11

## Constraints
- n == nums.length
- 1 <= n <= 5000
- -5000 <= nums[i] <= 5000
- All the integers of nums are unique.

## Solution
```java
class Solution {
	public int findMin(int[] nums) {
		int left=0,right=nums.length-1;

		while(left<right){
			int mid=left+(right-left)/2;

			if(nums[mid]>nums[right]){
				left=mid+1;
			}else{
				right=mid;
			}
		}
		return nums[left];
	}
}
```

Approach

- Use binary search to find the minimum in a rotated sorted array.
- Compare the middle element with the rightmost element:
  - If mid > right, the minimum is to the right of mid.
  - Otherwise, the minimum is at mid or to the left.
- Continue until left == right, which will be the index of the minimum element.

Time Complexity

O(log n), where n is the length of `nums`.

Space Complexity

O(1), as the search is performed in-place.
