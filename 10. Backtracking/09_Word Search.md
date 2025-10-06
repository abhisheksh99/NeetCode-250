# Word Search

**Difficulty:** Medium

## Problem Statement
Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example:**
```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true

Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true

Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false
```

**Constraints:**
- m == board.length
- n == board[i].length
- 1 <= m, n <= 6
- 1 <= word.length <= 15
- board and word consists of only lowercase and uppercase English letters.

---

## Java Solution
```java
public class Solution {
	public boolean exist(char[][] board, String word) {
		int m = board.length;
		int n = board[0].length;

		boolean[][] visited = new boolean[m][n];
		boolean result = false;
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (board[i][j] == word.charAt(0)) {
					result = backtrack(board, word, visited, i, j, 0);
					if (result) return true;
				}
			}
		}
		return false;
	}

	private boolean backtrack(char[][] board, String word, boolean[][] visited, int i, int j, int index) {
		if (index == word.length()) {
			return true;
		}
		if (i < 0 || i >= board.length || j < 0 || j >= board[0].length || visited[i][j] || board[i][j] != word.charAt(index)) {
			return false;
		}
		visited[i][j] = true;
		if (backtrack(board, word, visited, i + 1, j, index + 1) ||
			backtrack(board, word, visited, i - 1, j, index + 1) ||
			backtrack(board, word, visited, i, j + 1, index + 1) ||
			backtrack(board, word, visited, i, j - 1, index + 1)) {
			return true;
		}
		visited[i][j] = false;
		return false;
	}
}
```

---

## Approach
- For each cell in the board, start a backtracking search if the cell matches the first character of the word.
- Use a visited matrix to avoid revisiting the same cell in the current path.
- Recursively search in all four directions (up, down, left, right).
- If the full word is matched, return true; otherwise, backtrack.

---

## Time and Space Complexity
- **Time Complexity:** O(m * n * 3^L), where m and n are the dimensions of the board and L is the length of the word.
- **Space Complexity:** O(L) for the recursion stack and O(m * n) for the visited matrix.
