# 238. Product of Array Except Self
**Medium**

## Problem Statement
Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.
The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.
You must write an algorithm that runs in O(n) time and without using the division operation.

## Examples
### Example 1:
- Input: nums = [1,2,3,4]
- Output: [24,12,8,6]

### Example 2:
- Input: nums = [-1,1,0,-3,3]
- Output: [0,0,9,0,0]

## Constraints
- 2 <= nums.length <= 10^5
- -30 <= nums[i] <= 30
- The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.

## Solution
```java
class Solution {
		public int[] productExceptSelf(int[] nums) {
				int result[]=new int[nums.length];
				int pre=1,post=1;

				for(int i=0;i<nums.length;i++){
						result[i]=pre;
						pre=pre*nums[i];
				}

				for(int i=nums.length-1;i>=0;i--){
						result[i]=post*result[i];
						post=post*nums[i];

				}
				return result;
		}
}  
```

Approach

- Initialize a result array of the same length as nums.
- Traverse the array from left to right:
	- For each index, set result[i] to the product of all elements to the left (prefix product).
	- Update the prefix product as you go.
- Traverse the array from right to left:
	- For each index, multiply result[i] by the product of all elements to the right (postfix product).
	- Update the postfix product as you go.
- Notes: No division is used; only two passes and constant extra space (excluding output array).

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(1), excluding the output array.
