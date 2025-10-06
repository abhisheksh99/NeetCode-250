# 1921. Concatenation of Array
**Easy**

## Problem Statement
Given an integer array `nums` of length n, return an array `ans` of length `2 * n` where:
- `ans[i] == nums[i]` for 0 <= i < n
- `ans[i + n] == nums[i]` for 0 <= i < n

## Examples
### Example 1:
- Input: `nums = [1,2,1]`
- Output: `[1,2,1,1,2,1]`

### Example 2:
- Input: `nums = [1,3,2,1]`
- Output: `[1,3,2,1,1,3,2,1]`

## Constraints
- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`

## Solution
```java
class Solution {
	public int[] getConcatenation(int[] nums) {
		int[] ans = new int[2*nums.length];

		for(int i=0;i<nums.length;i++){
			ans[i]=nums[i];
			ans[i+nums.length]=nums[i];
		}
		return ans;
	}
}
```


Approach

- Let n = nums.length.
- Allocate an answer array `ans` of size 2 * n.
- Iterate i from 0 to n-1:
	- Set `ans[i] = nums[i]`.
	- Set `ans[i + n] = nums[i]`.
- Return `ans`.
- Notes: single-pass, no extra data structures; O(n) time and O(n) space.
- Edge cases: handles n = 1 fine; if `nums` could be null, validate before proceeding (LeetCode constraints guarantee length ≥ 1).

Time Complexity

O(n) — one pass to fill the result array.

Space Complexity

O(n) — the output array has length 2n.

