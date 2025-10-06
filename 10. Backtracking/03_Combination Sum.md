# Combination Sum

**Difficulty:** Medium

## Problem Statement
Given an array of distinct integers `candidates` and a target integer `target`, return a list of all unique combinations of `candidates` where the chosen numbers sum to `target`. You may return the combinations in any order.

The same number may be chosen from `candidates` an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

**Example:**
```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]

Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5],[2,2,3,3]]
```

**Constraints:**
- 1 <= candidates.length <= 30
- 2 <= candidates[i] <= 40
- All elements of candidates are distinct.
- 1 <= target <= 40

---

## Java Solution
```java
class Solution {
	public List<List<Integer>> combinationSum(int[] candidates, int target) {
		List<List<Integer>> res = new ArrayList<List<Integer>>();
		List<Integer> combination = new ArrayList<>();
		backTrack(target, res, combination, 0, candidates);
		return res;
	}
	public void backTrack(int target, List<List<Integer>> res, List<Integer> combination, 
						  int start, int[] candidates) {
		if (target == 0) {
			res.add(new ArrayList<Integer>(combination));
		} else if (target < 0) {
			return;
		}
		for (int i = start; i < candidates.length; i++) {
			combination.add(candidates[i]);
			backTrack(target - candidates[i], res, combination, i, candidates);
			combination.remove(combination.size() - 1);
		}
	}
}
```

---

## Approach
- Use backtracking to explore all possible combinations that sum to the target.
- At each step, try including each candidate (starting from the current index to allow unlimited use).
- If the target becomes 0, add the current combination to the result.
- If the target becomes negative, backtrack.

---

## Time and Space Complexity
- **Time Complexity:** O(2^t), where t is the target value (since each candidate can be chosen unlimited times).
- **Space Complexity:** O(t) for the recursion stack and O(k) for storing all valid combinations.
