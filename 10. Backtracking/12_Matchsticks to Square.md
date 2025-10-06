# Matchsticks to Square

**Difficulty:** Medium

## Problem Statement
You are given an integer array matchsticks where matchsticks[i] is the length of the i-th matchstick. You want to use all the matchsticks to make one square. You should not break any stick, but you can link them up, and each matchstick must be used exactly one time.

Return true if you can make this square and false otherwise.

**Example:**
```
Input: matchsticks = [1,1,2,2,2]
Output: true
Explanation: You can form a square with length 2.

Input: matchsticks = [3,3,3,3,4]
Output: false
```

**Constraints:**
- 1 <= matchsticks.length <= 15
- 1 <= matchsticks[i] <= 10^8

---

## Java Solution
```java
class Solution {
	public boolean makesquare(int[] matchsticks) {
		int total = 0;
		for (int len : matchsticks) total += len;
		if (total % 4 != 0) return false;
		int target = total / 4;
		Arrays.sort(matchsticks);
		return backtrack(matchsticks, matchsticks.length - 1, 0, 0, 0, 0, target);
	}

	private boolean backtrack(int[] sticks, int index, int top, int bottom, int left, int right, int target) {
		if (index < 0)
			return top == target && bottom == target && left == target && right == target;
		int val = sticks[index];
		if (top > target || bottom > target || left > target || right > target)
			return false;
		if (backtrack(sticks, index - 1, top + val, bottom, left, right, target)) return true;
		if (backtrack(sticks, index - 1, top, bottom + val, left, right, target)) return true;
		if (backtrack(sticks, index - 1, top, bottom, left + val, right, target)) return true;
		if (backtrack(sticks, index - 1, top, bottom, left, right + val, target)) return true;
		return false;
	}
}
```

---

## Approach
- Check if the total length of matchsticks is divisible by 4 (each side of the square must be equal).
- Sort the matchsticks in descending order for faster pruning.
- Use backtracking to try placing each matchstick on one of the four sides.
- Prune the search if any side exceeds the target length.
- Return true if all four sides are equal to the target length when all matchsticks are used.

---

## Time and Space Complexity
- **Time Complexity:** O(4^n), where n is the number of matchsticks (each stick can go to any of 4 sides).
- **Space Complexity:** O(n) for the recursion stack.
