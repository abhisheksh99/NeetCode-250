# 36. Valid Sudoku
**Medium**

## Problem Statement
Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:
- Each row must contain the digits 1-9 without repetition.
- Each column must contain the digits 1-9 without repetition.
- Each of the nine 3x3 sub-boxes must contain the digits 1-9 without repetition.

## Examples
### Example 1:
```
Input:
board = [
  ["5","3",".",".","7",".",".",".","."]
  ["6",".",".","1","9","5",".",".","."]
  [".","9","8",".",".",".",".","6","."]
  ["8",".",".",".","6",".",".",".","3"]
  ["4",".",".","8",".","3",".",".","1"]
  ["7",".",".",".","2",".",".",".","6"]
  [".","6",".",".",".",".","2","8","."]
  [".",".",".","4","1","9",".",".","5"]
  [".",".",".",".","8",".",".","7","9"]
]
Output: true
```

## Constraints
- board.length == 9
- board[i].length == 9
- board[i][j] is a digit 1-9 or '.'.

## Solution
```java
class Solution {
	public boolean isValidSudoku(char[][] board) {
		int N = 9;
        
		HashSet<Character>[] rows = new HashSet[N];
		HashSet<Character>[] cols = new HashSet[N];
		HashSet<Character>[] boxes = new HashSet[N];
        
		for (int i = 0; i < N; i++) {
			rows[i] = new HashSet<>();
			cols[i] = new HashSet<>();
			boxes[i] = new HashSet<>();
		}

		for (int i = 0; i < N; i++) {
			for (int j = 0; j < N; j++) {
				char val = board[i][j];
                
				if (val == '.') {
					continue;
				}

				// Check row
				if (rows[i].contains(val)) {
					return false;
				}
				rows[i].add(val);

				// Check column
				if (cols[j].contains(val)) {
					return false;
				}
				cols[j].add(val);

				// Check box
				int idx = (i / 3) * 3 + (j / 3);
				if (boxes[idx].contains(val)) {
					return false;
				}
				boxes[idx].add(val);
			}
		}

		return true;
	}
}
```

Approach

- Use three arrays of HashSet to track seen digits for each row, column, and 3x3 box.
- For each cell in the board:
  - If the cell is '.', skip it.
  - If the digit is already in the corresponding row, column, or box set, return false.
  - Otherwise, add the digit to the respective sets.
- If no duplicates are found after checking all cells, return true.
- Notes: The box index is calculated as (i / 3) * 3 + (j / 3).

Time Complexity

O(1), since the board size is fixed (9x9).

Space Complexity

O(1), as the extra space used is constant for a 9x9 board.
