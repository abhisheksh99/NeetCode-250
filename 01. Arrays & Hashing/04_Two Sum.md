# 1. Two Sum
**Easy**

## Problem Statement
Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
You can return the answer in any order.

## Examples
### Example 1:
- Input: nums = [2,7,11,15], target = 9
- Output: [0,1]

### Example 2:
- Input: nums = [3,2,4], target = 6
- Output: [1,2]

### Example 3:
- Input: nums = [3,3], target = 6
- Output: [0,1]

## Constraints
- 2 <= nums.length <= 10^4
- -10^9 <= nums[i] <= 10^9
- -10^9 <= target <= 10^9
- Only one valid answer exists.

## Solution
```java
class Solution {
	public int[] twoSum(int[] nums, int target) {
		HashMap<Integer,Integer> map = new HashMap<>();

		for(int i=0;i<nums.length;i++){
			int complement = target -nums[i];
			if(map.containsKey(complement)){
				return new int[] {map.get(complement),i};
			}
			map.put(nums[i],i);
		}
		return new int[]{};
	}
}
```

Approach

- Initialize an empty HashMap to store value-to-index mappings.
- Iterate through the array:
  - For each element, compute its complement with respect to the target.
  - If the complement exists in the map, return the indices (complement's index and current index).
  - Otherwise, add the current value and its index to the map.
- If no solution is found by the end of the loop, return an empty array (problem guarantees one solution exists).
- Notes: Single pass, uses a HashMap for O(1) lookups.

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(n), for the HashMap storing up to n elements.
