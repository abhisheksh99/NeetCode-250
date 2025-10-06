# Combination Sum II

**Difficulty:** Medium

## Problem Statement
Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sum to target. Each number in candidates may only be used once in the combination.

Note: The solution set must not contain duplicate combinations.

**Example:**
```
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: [[1,1,6],[1,2,5],[1,7],[2,6]]

Input: candidates = [2,5,2,1,2], target = 5
Output: [[1,2,2],[5]]
```

**Constraints:**
- 1 <= candidates.length <= 100
- 1 <= candidates[i] <= 50
- 1 <= target <= 30

---

## Java Solution
```java
class Solution {
	public List<List<Integer>> combinationSum2(int[] candidates, int target) {
		List<List<Integer>> result = new ArrayList<>();
		Arrays.sort(candidates); // Sort the array to handle duplicates
		backtrack(result, new ArrayList<>(), candidates, target, 0);
		return result;
	}

	private void backtrack(List<List<Integer>> result, List<Integer> current, int[] candidates, int target, int start) {
		if (target == 0) {
			result.add(new ArrayList<>(current));
			return;
		}

		for (int i = start; i < candidates.length; i++) {
			// Skip duplicates
			if (i > start && candidates[i] == candidates[i - 1]) {
				continue;
			}
			// Early termination if the remaining sum becomes negative
			if (target - candidates[i] < 0) {
				break;
			}
			current.add(candidates[i]);
			backtrack(result, current, candidates, target - candidates[i], i + 1);
			current.remove(current.size() - 1);
		}
	}
}
```

---

## Approach
- Sort the candidates array to handle duplicates easily.
- Use backtracking to explore all possible combinations.
- At each step, skip duplicate elements to avoid duplicate combinations.
- Only use each element once per combination (move to i+1 in recursion).
- Stop recursion early if the remaining target becomes negative.

---

## Time and Space Complexity
- **Time Complexity:** O(2^n), where n is the number of candidates (since each element can be either included or excluded).
- **Space Complexity:** O(n) for the recursion stack and O(k) for storing all valid combinations.
