## Backtracking + Recursion Template (Java)
```java
void backtrack(int[] input, int idx, List<Integer> path) {
	if (base case) {
		// process or collect result
		return;
	}
	for (int i = idx; i < input.length; i++) {
		// make a choice
		path.add(input[i]);
		backtrack(input, i + 1, path); // or i, or 0, depending on problem
		path.remove(path.size() - 1); // undo the choice (backtrack)
	}
}
```

## Plain Recursion Template (Java)
```java
int recurse(int n) {
	if (base case) {
		return result;
	}
	// recursive calls
	int left = recurse(n - 1);
	int right = recurse(n - 2);
	// combine results
	return left + right;
}
```
# Backtracking - Complete Guide & Patterns

## What is Backtracking?
Backtracking is a general algorithmic technique for solving problems recursively by trying to build a solution incrementally, one piece at a time, and removing those solutions that fail to satisfy the constraints of the problem at any point (i.e., backtrack).

## Common Backtracking Patterns

### 1. Subsets (Power Set)
```java
void backtrack(List<List<Integer>> res, List<Integer> temp, int[] nums, int start) {
	res.add(new ArrayList<>(temp));
	for (int i = start; i < nums.length; i++) {
		temp.add(nums[i]);
		backtrack(res, temp, nums, i + 1);
		temp.remove(temp.size() - 1);
	}
}
```

### 2. Permutations
```java
void backtrack(List<List<Integer>> res, List<Integer> temp, int[] nums, boolean[] used) {
	if (temp.size() == nums.length) {
		res.add(new ArrayList<>(temp));
		return;
	}
	for (int i = 0; i < nums.length; i++) {
		if (used[i]) continue;
		used[i] = true;
		temp.add(nums[i]);
		backtrack(res, temp, nums, used);
		used[i] = false;
		temp.remove(temp.size() - 1);
	}
}
```

### 3. Combinations
```java
void backtrack(List<List<Integer>> res, List<Integer> temp, int n, int k, int start) {
	if (temp.size() == k) {
		res.add(new ArrayList<>(temp));
		return;
	}
	for (int i = start; i <= n; i++) {
		temp.add(i);
		backtrack(res, temp, n, k, i + 1);
		temp.remove(temp.size() - 1);
	}
}
```

### 4. N-Queens (Board Problems)
```java
void solve(int row, int n, char[][] board, List<List<String>> res) {
	if (row == n) {
		res.add(construct(board));
		return;
	}
	for (int col = 0; col < n; col++) {
		if (isValid(board, row, col, n)) {
			board[row][col] = 'Q';
			solve(row + 1, n, board, res);
			board[row][col] = '.';
		}
	}
}
```

### 5. Palindrome Partitioning
```java
void backtrack(List<List<String>> res, List<String> temp, String s, int start) {
	if (start == s.length()) {
		res.add(new ArrayList<>(temp));
		return;
	}
	for (int end = start + 1; end <= s.length(); end++) {
		String sub = s.substring(start, end);
		if (isPalindrome(sub)) {
			temp.add(sub);
			backtrack(res, temp, s, end);
			temp.remove(temp.size() - 1);
		}
	}
}
```

---

## Backtracking Technique
- Use recursion to explore all possible choices at each step.
- Make a choice, recurse, then undo the choice (backtrack).
- Prune invalid choices early to improve efficiency.
- Often used for problems involving combinations, permutations, subsets, and constraint satisfaction (e.g., Sudoku, N-Queens).

## General Backtracking Template (Java)
```java
void backtrack(PartialSolution state) {
	if (isSolution(state)) {
		// process solution
		return;
	}
	for (choice : choices) {
		if (isValid(choice, state)) {
			makeChoice(state, choice);
			backtrack(state);
			undoChoice(state, choice);
		}
	}
}
```

---

## Time and Space Complexity
- **Time Complexity:**
  - Backtracking is often exponential in time, e.g., O(2^n) for subsets, O(n!) for permutations, O(k^n) for board problems.
  - Pruning and constraints can reduce the search space.
- **Space Complexity:**
  - O(n) for recursion stack (where n is the depth of the recursion).
  - Additional space for storing solutions (e.g., O(2^n) for all subsets).

---

## Tips for Interviews
- Always undo your choice after recursion (backtrack step).
- Use additional data structures (e.g., boolean[] used) to track state.
- Prune invalid paths early for efficiency.
- Draw recursion trees for understanding.
- Practice classic problems: Subsets, Permutations, Combinations, N-Queens, Sudoku, Word Search.
