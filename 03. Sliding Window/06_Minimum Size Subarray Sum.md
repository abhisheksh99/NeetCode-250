# 209. Minimum Size Subarray Sum
**Medium**

## Problem Statement
Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a subarray whose sum is greater than or equal to `target`. If there is no such subarray, return 0 instead.

## Examples
### Example 1:
- Input: target = 7, nums = [2,3,1,2,4,3]
- Output: 2
- Explanation: The subarray [4,3] has the minimal length under the problem constraint.

### Example 2:
- Input: target = 4, nums = [1,4,4]
- Output: 1

### Example 3:
- Input: target = 11, nums = [1,1,1,1,1,1,1,1]
- Output: 0

## Constraints
- 1 <= target <= 10^9
- 1 <= nums.length <= 10^5
- 1 <= nums[i] <= 10^4

## Solution
```java
class Solution {
	public int minSubArrayLen(int target, int[] nums) {
		int low = 0, high = 0;
		int minLenWindow = Integer.MAX_VALUE;
		int currSum = 0;

		for (high = 0; high < nums.length; high++) {
			currSum += nums[high];
			while (currSum >= target) {
				int currWindow = high - low + 1;
				minLenWindow = Math.min(minLenWindow, currWindow);
				currSum -= nums[low];
				low++;
			}
		}
		return minLenWindow == Integer.MAX_VALUE ? 0 : minLenWindow;
	}
}
```

Approach

- Use a sliding window with two pointers (`low` and `high`) to maintain the current subarray.
- Expand the window by moving `high` and adding `nums[high]` to the current sum.
- When the sum is greater than or equal to `target`, update the minimum window length and shrink the window from the left by moving `low` and subtracting `nums[low]`.
- Continue until the end of the array.
- Return 0 if no valid subarray is found.

Time Complexity

O(n), where n is the length of `nums`.

Space Complexity

O(1), as only a few variables are used for tracking the window and sum.
