# N-Queens II

**Difficulty:** Hard

## Problem Statement
The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return the number of distinct solutions to the n-queens puzzle.

**Example:**
```
Input: n = 4
Output: 2
```

**Constraints:**
- 1 <= n <= 9

---

## Java Solution
```java
class Solution {
    public int totalNQueens(int n) {
        char board[][] = new char[n][n];
        for (char i[] : board)
            Arrays.fill(i, '.');
        return dfs(0, board);
    }
    public int dfs(int col, char board[][]) {
        if (col == board.length) return 1;
        int count = 0;
        for (int row = 0; row < board.length; row++) {
            if (isSafe(board, row, col)) {
                board[row][col] = 'Q';
                count += dfs(col + 1, board);
                board[row][col] = '.';
            }
        }
        return count;
    }
    public boolean isSafe(char board[][], int row, int col) {
        int dupRow = row;
        int dupCol = col;
        while (row >= 0 && col >= 0) {
            if (board[row][col] == 'Q') return false;
            row--;
            col--;
        }
        row = dupRow;
        col = dupCol;
        while (col >= 0) {
            if (board[row][col] == 'Q') return false;
            col--;
        }
        row = dupRow;
        col = dupCol;
        while (col >= 0 && row < board.length) {
            if (board[row][col] == 'Q') return false;
            row++;
            col--;
        }
        return true;
    }
}
```

---

## Approach
- Use backtracking to try placing a queen in each column of the board.
- For each column, try every row and check if placing a queen is valid (no other queen in the same row, upper-left diagonal, or lower-left diagonal).
- If a valid position is found, place the queen and recurse for the next column.
- When all columns are filled, increment the count.

---

## Time and Space Complexity
- **Time Complexity:** O(n!), where n is the size of the board (since there are n! possible ways to place n queens).
- **Space Complexity:** O(n^2) for the board and O(n) for the recursion stack.
