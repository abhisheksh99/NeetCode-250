# Set Matrix Zeroes

**Difficulty:** Medium

## Problem Statement  
Given an `m x n` integer matrix, if an element is 0, set its entire row and column to 0. You must do it **in-place**, modifying the input matrix directly.

---

## Example  
Input:  
[
[1,1,1],
[1,0,1],
[1,1,1]
]



Output:  
[
[1,0,1],
[0,0,0],
[1,0,1]
]



---

## Constraints  
- `m == matrix.length`  
- `n == matrix[0].length`  
- `1 <= m, n <= 200`  
- `-2³¹ <= matrix[i][j] <= 2³¹ - 1`

---

## Approach  
- Use the first row and first column of the matrix as markers to indicate whether a row or column should be zeroed.  
- Traverse the matrix and whenever a zero is found:  
  - Mark the start of the row (`matrix[i][0]`) and the start of the column (`matrix[0][j]`) as zero.  
- Maintain a boolean flag to remember if the first column itself contains any zero (since `matrix[0][0]` overlaps both first row and first column).  
- Use the markers to update the matrix cells to zero, excluding the first row and first column initially.  
- Finally, update the first row and first column based on the markers and the flag.

---

## Time and Space Complexity  
- **Time Complexity:** O(m * n), as we traverse the matrix a few times.  
- **Space Complexity:** O(1), because the solution modifies the matrix in-place without extra space.

---

## Code

```java
public class Solution {
    public void setZeroes(int[][] matrix) {
        boolean firstcol = false;
        int r = matrix.length;
        int c = matrix[0].length;
        
        // Use first row and column as markers.
        for (int i = 0; i < r; i++) {
            if (matrix[i][0] == 0) {
                firstcol = true;
            }
            for (int j = 1; j < c; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        
        // Use markers to set zeroes for cells excluding first row and column.
        for (int i = 1; i < r; i++) {
            for (int j = 1; j < c; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        
        // If first row needs to be zeroed.
        if (matrix[0][0] == 0) {
            for (int j = 0; j < c; j++) {
                matrix[0][j] = 0;
            }
        }
        
        // If first column needs to be zeroed.
        if (firstcol) {
            for (int i = 0; i < r; i++) {
                matrix[i][0] = 0;
            }
        }
    }
}
```