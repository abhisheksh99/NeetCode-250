# 410. Split Array Largest Sum
**Hard**

## Problem Statement
Given an array `nums` which consists of non-negative integers and an integer `k`, split the array into `k` non-empty continuous subarrays. Minimize the largest sum among these subarrays.

Return the minimized largest sum of the split.

## Examples
### Example 1:
- Input: nums = [7,2,5,10,8], k = 2
- Output: 18
- Explanation: There are four ways to split nums into two subarrays. The best way is to split it into [7,2,5] and [10,8], where the largest sum among the two subarrays is only 18.

### Example 2:
- Input: nums = [1,2,3,4,5], k = 2
- Output: 9

## Constraints
- 1 <= nums.length <= 1000
- 0 <= nums[i] <= 10^6
- 1 <= k <= min(50, nums.length)

## Solution
```java
class Solution {

	// Helper function to check if we can split nums into at most k subarrays
	// with each subarray sum ≤ target
	private boolean canSplit(int[] nums, int k, int target) {
		int currentSum = 0, count = 1;
		for (int num : nums) {
			currentSum += num;
			if (currentSum > target) {
				// Start new subarray
				currentSum = num;
				count++;
				if (count > k) return false;
			}
		}
		return true;
	}

	public int splitArray(int[] nums, int k) {
		int left = 0, right = 0;
		for (int num : nums) {
			left = Math.max(left, num); // max element
			right += num;               // sum of all elements
		}

		while (left < right) {
			int mid = left + (right - left) / 2;
			if (canSplit(nums, k, mid)) {
				right = mid;  // try to find smaller max sum
			} else {
				left = mid + 1;  // increase max sum
			}
		}
		return left;  // minimum largest sum possible
	}
}
```

Approach

- Use binary search on the answer (minimum largest subarray sum).
- The search range is from the maximum element to the sum of all elements in the array.
- For each candidate sum, check if the array can be split into at most k subarrays with each sum ≤ candidate.
- If possible, try a smaller sum; otherwise, increase the sum.
- Return the minimum largest sum found.

Time Complexity

O(n log S), where n is the length of nums and S is the sum of all elements in nums.

Space Complexity

O(1), as only a few variables are used.
