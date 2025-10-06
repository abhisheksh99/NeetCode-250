# Partition to K Equal Sum Subsets

**Difficulty:** Medium

## Problem Statement
Given an array of integers nums and an integer k, return true if it is possible to divide this array into k non-empty subsets whose sums are all equal.

**Example:**
```
Input: nums = [4,3,2,3,5,2,1], k = 4
Output: true
Explanation: It's possible to divide it into 4 subsets (5), (1,4), (2,3), (2,3) with equal sum.
```

**Constraints:**
- 1 <= k <= nums.length <= 16
- 1 <= nums[i] <= 10^4
- The sum of the elements of nums is in the range [1, 10^6].

---

## Java Solution
```java
public class Solution {
	public boolean canPartitionKSubsets(int[] nums, int k) {
		int totalSum = 0;
		for (int num : nums) totalSum += num;
		if (totalSum % k != 0) return false;  // can't divide evenly
		int target = totalSum / k;
		Arrays.sort(nums);
		reverse(nums); // Sort descending to optimize pruning
		boolean[] visited = new boolean[nums.length];
		return backtrack(nums, visited, k, 0, 0, target);
	}
	private boolean backtrack(int[] nums, boolean[] visited, int k, int start, int currentSum, int target) {
		if (k == 0) return true;  // all subsets formed
		if (currentSum == target)
			// current subset complete, go for the next subset
			return backtrack(nums, visited, k - 1, 0, 0, target);
		for (int i = start; i < nums.length; i++) {
			if (!visited[i] && currentSum + nums[i] <= target) {
				visited[i] = true;
				if (backtrack(nums, visited, k, i + 1, currentSum + nums[i], target))
					return true;
				visited[i] = false;
			}
		}
		return false;
	}
	private void reverse(int[] nums) {
		int left = 0, right = nums.length - 1;
		while (left < right) {
			int temp = nums[left];
			nums[left] = nums[right];
			nums[right] = temp;
			left++;
			right--;
		}
	}
}
```

---

## Approach
- Check if the total sum is divisible by k; if not, return false.
- Sort the array in descending order for faster pruning.
- Use backtracking to try to fill each subset to the target sum.
- Use a visited array to track which elements are used.
- When a subset reaches the target, start filling the next subset.
- Return true if all k subsets are formed with equal sum.

---

## Time and Space Complexity
- **Time Complexity:** O(k * 2^n), where n is the length of nums (since each element can be included/excluded in each subset).
- **Space Complexity:** O(n) for the recursion stack and visited array.
