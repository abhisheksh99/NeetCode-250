# Permutations

**Difficulty:** Medium

## Problem Statement
Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

**Example:**
```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**Constraints:**
- 1 <= nums.length <= 6
- -10 <= nums[i] <= 10
- All the integers of nums are unique.

---

## Java Solution
```java
class Solution {
	public List<List<Integer>> permute(int[] nums) {
		List<List<Integer>> result = new ArrayList<>();
		boolean[] used = new boolean[nums.length];
		backtrack(result, new ArrayList<>(), nums, used);
		return result;
	}
	private void backtrack(List<List<Integer>> result, List<Integer> current, int[] nums, boolean[] used) {
		if (current.size() == nums.length) {
			result.add(new ArrayList<>(current));
			return;
		}
		for (int i = 0; i < nums.length; i++) {
			if (!used[i]) {
				current.add(nums[i]);
				used[i] = true;
				backtrack(result, current, nums, used);
				used[i] = false;
				current.remove(current.size() - 1);
			}
		}
	}
}
```

---

## Approach
- Use backtracking to generate all possible permutations.
- At each step, try each unused number, mark it as used, and recurse.
- When the current permutation is complete, add it to the result.

---

## Time and Space Complexity
- **Time Complexity:** O(n!), where n is the length of nums (since there are n! permutations).
- **Space Complexity:** O(n!) for storing all permutations and O(n) for the recursion stack and used array.
