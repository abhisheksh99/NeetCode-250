# 41. First Missing Positive
**Hard**

## Problem Statement
Given an unsorted integer array `nums`, find the smallest missing positive integer.
You must implement an algorithm that runs in O(n) time and uses constant extra space.

## Examples
### Example 1:
- Input: nums = [1,2,0]
- Output: 3

### Example 2:
- Input: nums = [3,4,-1,1]
- Output: 2

### Example 3:
- Input: nums = [7,8,9,11,12]
- Output: 1

## Constraints
- 1 <= nums.length <= 5 * 10^5
- -2^31 <= nums[i] <= 2^31 - 1

## Solution
```java
class Solution {
	public int firstMissingPositive(int[] nums) {
		int n = nums.length;
		boolean hasOne = false;
		// Step 1: Check for 1
		for (int num : nums) {
			if (num == 1) {
				hasOne = true;
				break;
			}
		}
		if (!hasOne) return 1;
		// Step 2: Replace negatives, zeros, and >n with 1
		for (int i = 0; i < n; i++) {
			if (nums[i] <= 0 || nums[i] > n) nums[i] = 1;
		}
		// Step 3: Use index as marker
		for (int i = 0; i < n; i++) {
			int a = Math.abs(nums[i]);
			if (a == n) {
				nums[0] = -Math.abs(nums[0]);
			} else {
				nums[a] = -Math.abs(nums[a]);
			}
		}
		// Step 4: Find the first missing
		for (int i = 1; i < n; i++) {
			if (nums[i] > 0) return i;
		}
		if (nums[0] > 0) return n;
		return n + 1;
	}
}
```

Approach

- Check if 1 is present in the array; if not, return 1.
- Replace all non-positive numbers and numbers greater than n with 1.
- Use the index as a marker: for each value a in the array, mark the presence of a by making nums[a] negative.
- Special case for n: use nums[0] as a marker for the value n.
- After marking, the first positive index indicates the missing positive integer.
- If all indices are negative, return n + 1.
- Notes: This approach uses the input array itself for marking, achieving O(1) extra space.

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(1), as the marking is done in-place.
