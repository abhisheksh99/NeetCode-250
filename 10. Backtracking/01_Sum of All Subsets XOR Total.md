# Sum of All Subsets XOR Total

**Difficulty:** Easy

## Problem Statement
Given an integer array `nums`, return the sum of all XOR totals for every subset of `nums`.

The XOR total of a subset is the bitwise XOR of all its elements. The XOR total of an empty subset is 0.

**Example:**
```
Input: nums = [1,3]
Output: 6
Explanation: The subsets are [ ], [1], [3], [1,3].
Their XOR totals are 0, 1, 3, 1^3 = 2.
Sum: 0 + 1 + 3 + 2 = 6
```

**Constraints:**
- 1 <= nums.length <= 12
- 1 <= nums[i] <= 20

---

## Java Solution
```java
public class Solution {
	int res = 0;

	public int subsetXORSum(int[] nums) {
		backtrack(0, nums, new ArrayList<>());
		return res;
	}

	private void backtrack(int i, int[] nums, List<Integer> subset) {
		int xorr = 0;
		for (int num : subset) xorr ^= num;
		res += xorr;

		for (int j = i; j < nums.length; j++) {
			subset.add(nums[j]);
			backtrack(j + 1, nums, subset);
			subset.remove(subset.size() - 1);
		}
	}
}
```

---

## Approach
- Use backtracking to generate all possible subsets of the array.
- For each subset, calculate the XOR of its elements and add it to the result.
- The empty subset is included and its XOR is 0.

---

## Time and Space Complexity
- **Time Complexity:** O(n * 2^n), where n is the length of nums (since there are 2^n subsets and each subset can take up to O(n) to compute the XOR).
- **Space Complexity:** O(n) for the recursion stack and subset list.
