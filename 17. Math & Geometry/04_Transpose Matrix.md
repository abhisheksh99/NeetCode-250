# Transpose Matrix

**Difficulty:** Easy

## Problem Statement  
Given a 2D integer matrix, return the transpose of the matrix.

The transpose of a matrix is the matrix flipped over its main diagonal, switching the matrix's row and column indices.

**Example:**  
Input: matrix = [[1, 2, 3], [4, 5, 6]]  
Output: [[1, 4], [2, 5], [3, 6]]

**Constraints:**  
- `m == matrix.length`  
- `n == matrix[i].length`  
- 1 <= m, n <= 1000  
- 1 <= matrix[i][j] <= 10^9

---

## Approach  
- Initialize a new matrix `res` with dimensions [number_of_columns][number_of_rows].  
- Iterate through each element of the original matrix.  
- Assign `res[column][row] = matrix[row][column]` to flip rows and columns.  
- Return the transposed matrix.

---

## Time and Space Complexity  
- **Time Complexity:** O(m * n), where m and n are dimensions of the matrix.  
- **Space Complexity:** O(m * n) for the result matrix.

```java
public class Solution {
    public int[][] transpose(int[][] matrix) {
        int ROWS = matrix.length, COLS = matrix[0].length;
        int[][] res = new int[COLS][ROWS];

        for (int r = 0; r < ROWS; r++) {
            for (int c = 0; c < COLS; c++) {
                res[c][r] = matrix[r][c];
            }
        }

        return res;
    }
}
```