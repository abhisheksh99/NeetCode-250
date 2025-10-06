# 167. Two Sum II - Input Array Is Sorted
**Medium**

## Problem Statement
Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number. Return the indices of the two numbers (1-indexed) as an integer array answer of size 2.
You may not use the same element twice.

## Examples
### Example 1:
- Input: numbers = [2,7,11,15], target = 9
- Output: [1,2]

### Example 2:
- Input: numbers = [2,3,4], target = 6
- Output: [1,3]

### Example 3:
- Input: numbers = [-1,0], target = -1
- Output: [1,2]

## Constraints
- 2 <= numbers.length <= 3 * 10^4
- -1000 <= numbers[i] <= 1000
- numbers is sorted in non-decreasing order.
- 1 <= target <= 10^9

## Solution
```java
class Solution {
	public int[] twoSum(int[] numbers, int target) {
		int left=0,right=numbers.length-1;
		while(left<right){
			int currSum = numbers[left]+numbers[right];
			if(currSum==target){
				return new int[]{left+1,right+1};
			}

			if(currSum<target){
				left++;
			}else{
				right--;
			}
		}
		return new int[]{};
        
	}
}
```

Approach

- Use two pointers, `left` at the start and `right` at the end of the array.
- Calculate the sum of the elements at `left` and `right`.
- If the sum equals the target, return the 1-based indices.
- If the sum is less than the target, move `left` forward; if greater, move `right` backward.
- Continue until the pair is found.
- Notes: This approach leverages the sorted property for O(n) time.

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(1), as only two pointers are used.
