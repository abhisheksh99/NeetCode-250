# Subsets II

**Difficulty:** Medium

## Problem Statement
Given an integer array nums that may contain duplicates, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

**Example:**
```
Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]
```

**Constraints:**
- 1 <= nums.length <= 10
- -10 <= nums[i] <= 10

---

## Java Solution
```java
class Solution {
	public List<List<Integer>> subsetsWithDup(int[] nums) {
		List<List<Integer>> result = new ArrayList<>();
		Arrays.sort(nums); // Sort the array to handle duplicates
		backtrack(result, new ArrayList<>(), nums, 0);
		return result;
	}

	private void backtrack(List<List<Integer>> result, List<Integer> current, int[] nums, int start) {
		result.add(new ArrayList<>(current));

		for (int i = start; i < nums.length; i++) {
			// Skip duplicates
			if (i > start && nums[i] == nums[i - 1]) {
				continue;
			}
			current.add(nums[i]);
			backtrack(result, current, nums, i + 1);
			current.remove(current.size() - 1);
		}
	}
}
```

---

## Approach
- Sort the input array to bring duplicates together.
- Use backtracking to generate all possible subsets.
- At each step, skip duplicate elements to avoid duplicate subsets.
- Add the current subset to the result, then recursively explore further elements.

---

## Time and Space Complexity
- **Time Complexity:** O(n * 2^n), where n is the length of nums (since there are up to 2^n subsets and each subset can take up to O(n) to copy).
- **Space Complexity:** O(n * 2^n) for storing all subsets.
