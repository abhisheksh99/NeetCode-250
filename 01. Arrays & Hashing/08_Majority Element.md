# 169. Majority Element
**Easy**

## Problem Statement
Given an array `nums` of size n, return the majority element.
The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

## Examples
### Example 1:
- Input: nums = [3,2,3]
- Output: 3

### Example 2:
- Input: nums = [2,2,1,1,1,2,2]
- Output: 2

## Constraints
- n == nums.length
- 1 <= n <= 5 * 10^4
- -10^9 <= nums[i] <= 10^9

## Solution
```java
class Solution {
	public int majorityElement(int[] nums) {
		int candidate=0;
		int count=0;

		for(int i=0;i<nums.length;i++){
			if(count==0){
				candidate=nums[i];
			}
			if(candidate==nums[i]){
				count++;
			}else{
				count--;
			}
		}
		return candidate;
	}
}
```

Approach

- Use the Boyer-Moore Voting Algorithm to find the majority element.
- Initialize `candidate` and `count` to 0.
- Iterate through the array:
  - If `count` is 0, set `candidate` to the current element.
  - If the current element equals `candidate`, increment `count`.
  - Otherwise, decrement `count`.
- After the loop, `candidate` will be the majority element.
- Notes: This works because the majority element always exists in the array.

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(1), as only two variables are used.
