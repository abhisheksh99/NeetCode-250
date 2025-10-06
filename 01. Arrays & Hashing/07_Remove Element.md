# 27. Remove Element
**Easy**

## Problem Statement
Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in-place. The order of the elements may be changed. Then return the number of elements in `nums` which are not equal to `val`.
It does not matter what you leave beyond the returned length.

## Examples
### Example 1:
- Input: nums = [3,2,2,3], val = 3
- Output: 2, nums = [2,2,_,_]

### Example 2:
- Input: nums = [0,1,2,2,3,0,4,2], val = 2
- Output: 5, nums = [0,1,4,0,3,_,_,_]

## Constraints
- 0 <= nums.length <= 100
- 0 <= nums[i] <= 50
- 0 <= val <= 100

## Solution
```java
class Solution {
	public int removeElement(int[] nums, int val) {
		int k=0;
		for(int i=0;i<nums.length;i++){
			if(nums[i]!=val){
				nums[k]=nums[i];
				k++;
			}
		}
		return k;
	}
}
```

Approach

- Initialize a pointer `k` to 0 to track the position for non-val elements.
- Iterate through the array:
  - If the current element is not equal to `val`, assign it to `nums[k]` and increment `k`.
- After the loop, the first `k` elements of `nums` are the elements not equal to `val`.
- Return `k` as the new length.
- Notes: In-place modification, order of elements can change, elements beyond `k` are irrelevant.

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(1), as the operation is done in-place.
