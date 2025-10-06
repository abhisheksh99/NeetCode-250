# N-Queens

**Difficulty:** Hard

## Problem Statement
The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return all distinct solutions to the n-queens puzzle. Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space, respectively.

**Example:**
```
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
```

**Constraints:**
- 1 <= n <= 9

---

## Java Solution
```java
class Solution {
    public List<List<String>> solveNQueens(int n) {
        char[][] board = new char[n][n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                board[i][j] = '.';
            }
        }
        List<List<String>> result = new ArrayList<>();
        backtrack(board, 0, result);
        return result;
    }
    private void backtrack(char[][] board, int col, List<List<String>> result) {
        if (col == board.length) {
            result.add(construct(board));
            return;
        }
        for (int i = 0; i < board.length; i++) {
            if (isValid(board, i, col)) {
                board[i][col] = 'Q';
                backtrack(board, col + 1, result);
                board[i][col] = '.';
            }
        }
    }
    private boolean isValid(char[][] board, int row, int col) {
        for (int i = 0; i < col; i++) {
            if (board[row][i] == 'Q') {
                return false;
            }
        }
        for (int i = row, j = col; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] == 'Q') {
                return false;
            }
        }
        for (int i = row, j = col; i < board.length && j >= 0; i++, j--) {
            if (board[i][j] == 'Q') {
                return false;
            }
        }
        return true;
    }
    private List<String> construct(char[][] board) {
        List<String> result = new ArrayList<>();
        for (int i = 0; i < board.length; i++) {
            String row = new String(board[i]);
            result.add(row);
        }
        return result;
    }
}
```

---

## Approach
- Use backtracking to try placing a queen in each column of the board.
- For each column, try every row and check if placing a queen is valid (no other queen in the same row, upper-left diagonal, or lower-left diagonal).
- If a valid position is found, place the queen and recurse for the next column.
- When all columns are filled, add the board configuration to the result.

---

## Time and Space Complexity
- **Time Complexity:** O(n!), where n is the size of the board (since there are n! possible ways to place n queens).
- **Space Complexity:** O(n^2) for the board and O(n) for the recursion stack.
