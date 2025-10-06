# Combinations

**Difficulty:** Medium

## Problem Statement
Given two integers n and k, return all possible combinations of k numbers chosen from the range 1 to n.

You may return the answer in any order.

**Example:**
```
Input: n = 4, k = 2
Output: [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
```

**Constraints:**
- 1 <= n <= 20
- 1 <= k <= n

---

## Java Solution
```java
public class Solution {
	private List<List<Integer>> res;

	public List<List<Integer>> combine(int n, int k) {
		res = new ArrayList<>();
		backtrack(1, n, k, new ArrayList<>());
		return res;
	}

	private void backtrack(int start, int n, int k, List<Integer> comb) {
		if (comb.size() == k) {
			res.add(new ArrayList<>(comb));
			return;
		}

		for (int i = start; i <= n; i++) {
			comb.add(i);
			backtrack(i + 1, n, k, comb);
			comb.remove(comb.size() - 1);
		}
	}
}
```

---

## Approach
- Use backtracking to generate all possible combinations of k numbers from 1 to n.
- At each step, add the next number to the current combination and recurse.
- When the combination reaches size k, add it to the result.

---

## Time and Space Complexity
- **Time Complexity:** O(C(n, k) * k), where C(n, k) is the number of combinations and k is the length of each combination.
- **Space Complexity:** O(C(n, k) * k) for storing all combinations and O(k) for the recursion stack.
