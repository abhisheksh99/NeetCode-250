# Permutations II

**Difficulty:** Medium

## Problem Statement
Given a collection of numbers, nums, that might contain duplicates, return all possible unique permutations in any order.

**Example:**
```
Input: nums = [1,1,2]
Output: [[1,1,2],[1,2,1],[2,1,1]]
```

**Constraints:**
- 1 <= nums.length <= 8
- -10 <= nums[i] <= 10

---

## Java Solution
```java
public class Solution {
	public List<List<Integer>> permuteUnique(int[] nums) {
		List<List<Integer>> result = new ArrayList<>();
		Arrays.sort(nums);  // Sort to handle duplicates
		boolean[] used = new boolean[nums.length];
		backtrack(nums, used, new ArrayList<>(), result);
		return result;
	}

	private void backtrack(int[] nums, boolean[] used, List<Integer> tempList, List<List<Integer>> result) {
		if (tempList.size() == nums.length) {
			result.add(new ArrayList<>(tempList));
			return;
		}
		for (int i = 0; i < nums.length; i++) {
			// Skip already used numbers
			if (used[i]) continue;

			// Skip duplicates: if current num is same as previous and previous is not used
			if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) continue;

			used[i] = true;
			tempList.add(nums[i]);

			backtrack(nums, used, tempList, result);

			used[i] = false;
			tempList.remove(tempList.size() - 1);
		}
	}
}
```

---

## Approach
- Sort the input array to bring duplicates together.
- Use backtracking to generate all possible unique permutations.
- At each step, skip used elements and skip duplicates by checking if the previous identical element was used.
- Add the current permutation to the result when complete.

---

## Time and Space Complexity
- **Time Complexity:** O(n! * n), where n is the length of nums (since there are up to n! unique permutations and each permutation takes O(n) to copy).
- **Space Complexity:** O(n! * n) for storing all permutations and O(n) for the recursion stack and used array.
