# Subsets

**Difficulty:** Medium

## Problem Statement
Given an integer array nums of unique elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

**Example:**
```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**Constraints:**
- 1 <= nums.length <= 10
- -10 <= nums[i] <= 10
- All the numbers of nums are unique.

---

## Java Solution
```java
class Solution {
	public List<List<Integer>> subsets(int[] nums) {
		List<List<Integer>> result = new ArrayList<>();
		generateSubsets(0, nums, new ArrayList<>(), result);
		return result;
	}
	private void generateSubsets(int index, int[] nums, List<Integer> current, List<List<Integer>> result) {
		result.add(new ArrayList<>(current));
		for (int i = index; i < nums.length; i++) {
			current.add(nums[i]);
			generateSubsets(i + 1, nums, current, result);
			current.remove(current.size() - 1);
		}
	}
}
```

---

## Approach
- Use backtracking to generate all possible subsets.
- At each step, decide whether to include the current element or not.
- Add the current subset to the result, then recursively explore further elements.

---

## Time and Space Complexity
- **Time Complexity:** O(n * 2^n), where n is the length of nums (since there are 2^n subsets and each subset can take up to O(n) to copy).
- **Space Complexity:** O(n * 2^n) for storing all subsets.
