# 130. Surrounded Regions

## Problem Statement
Given an `m x n` matrix `board` containing `'X'` and `'O'`, capture all regions that are 4-directionally surrounded by `'X'`.

A region is captured by flipping all `'O'`s into `'X'`s in that surrounded region.

### Examples

**Example 1:**
```
Input: board = [
  ["X","X","X","X"],
  ["X","O","O","X"],
  ["X","X","O","X"],
  ["X","O","X","X"]
]
Output: [
  ["X","X","X","X"],
  ["X","X","X","X"],
  ["X","X","X","X"],
  ["X","O","X","X"]
]
Explanation: The bottom-left 'O' is not surrounded by 'X' on all sides, so it remains as 'O'.
```

**Example 2:**
```
Input: board = [["X"]]
Output: [["X"]]
```

### Constraints:
- `m == board.length`
- `n == board[i].length`
- `1 <= m, n <= 200`
- `board[i][j]` is `'X'` or `'O'`.

## Solution
```java
public class Solution {
    public void solve(char[][] board) {
        if (board == null || board.length == 0) return;
        int rows = board.length, cols = board[0].length;

        // DFS from the borders to mark 'O's that shouldn't be flipped
        for (int r = 0; r < rows; r++) {
            dfs(board, r, 0);           // Left border
            dfs(board, r, cols - 1);    // Right border
        }
        for (int c = 0; c < cols; c++) {
            dfs(board, 0, c);           // Top border
            dfs(board, rows - 1, c);    // Bottom border
        }

        // Flip unmarked 'O's to 'X', and revert marked '*' to 'O'
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (board[r][c] == 'O') {
                    board[r][c] = 'X';
                } else if (board[r][c] == '*') {
                    board[r][c] = 'O';
                }
            }
        }
    }

    private void dfs(char[][] board, int row, int col) {
        int rows = board.length, cols = board[0].length;
        // Check boundaries and if current cell is 'O'
        if (row < 0 || col < 0 || row >= rows || col >= cols || board[row][col] != 'O') {
            return;
        }
        
        // Mark current cell as part of the border-connected region
        board[row][col] = '*';
        
        // Explore all four directions
        dfs(board, row + 1, col);  // Down
        dfs(board, row - 1, col);  // Up
        dfs(board, row, col + 1);  // Right
        dfs(board, row, col - 1);  // Left
    }
}
```

## Approach
1. **Border Processing**:
   - Perform DFS from all 'O's located on the borders of the board.
   - Mark these 'O's with a temporary marker ('*') to indicate they are connected to the border and should not be flipped.

2. **Board Update**:
   - After marking all border-connected 'O's, iterate through the entire board.
   - Flip all remaining 'O's to 'X' (these are the surrounded regions).
   - Revert the marked '*' back to 'O' (these are the regions connected to the border).

## Complexity
- **Time Complexity**: O(m × n), where m is the number of rows and n is the number of columns. Each cell is visited at most twice.
- **Space Complexity**: O(m × n) in the worst case due to the recursion stack for DFS when the entire board is filled with 'O's.

## Edge Cases
- Empty board or null input.
- Board with only one cell.
- Board where all cells are 'X' or all are 'O'.
- Board with 'O's only on the borders.
- Maximum size board (200x200).

## Key Insights
- The key observation is that any 'O' that is connected to the border (directly or indirectly) cannot be flipped to 'X'.
- By working from the borders inward, we can efficiently identify all regions that should remain 'O'.
- Using a temporary marker ('*') allows us to distinguish between 'O's that should be flipped and those that shouldn't in a single pass.
- The solution is efficient as it only requires two passes through the board and uses DFS for border processing.
- The space complexity is optimized by modifying the input board in-place rather than using additional data structures.