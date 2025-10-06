# 189. Rotate Array
**Medium**

## Problem Statement
Given an array, rotate the array to the right by `k` steps, where `k` is non-negative.

## Examples
### Example 1:
- Input: nums = [1,2,3,4,5,6,7], k = 3
- Output: [5,6,7,1,2,3,4]

### Example 2:
- Input: nums = [-1,-100,3,99], k = 2
- Output: [3,99,-1,-100]

## Constraints
- 1 <= nums.length <= 10^5
- -2^31 <= nums[i] <= 2^31 - 1
- 0 <= k <= 10^5

## Solution
```java
class Solution {
	public void rotate(int[] nums, int k) {
		int n = nums.length;
		k = k % n; // Normalize k

		reverse(nums, 0, n - 1);
		reverse(nums, 0, k - 1);
		reverse(nums, k, n - 1);
	}

	public static void reverse(int[] nums, int start, int end) {
		while (start < end) {
			int temp = nums[start];
			nums[start] = nums[end];
			nums[end] = temp;
			start++;
			end--;
		}
	}
}
```

Approach

- Normalize `k` to ensure it is within the array length.
- Reverse the entire array.
- Reverse the first `k` elements.
- Reverse the remaining `n - k` elements.
- This sequence of reversals rotates the array to the right by `k` steps in-place.

Time Complexity

O(n), where n is the length of the array (each element is moved at most twice).

Space Complexity

O(1), as the operation is done in-place.
