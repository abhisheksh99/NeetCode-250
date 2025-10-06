# Rotate Image

**Difficulty:** Medium

## Problem Statement  
You are given an n x n 2D matrix representing an image. Rotate the image by 90 degrees (clockwise) in-place.

You must rotate the matrix without allocating another 2D matrix.

**Example:**  
Input:  
[
[1, 2, 3],
[4, 5, 6],
[7, 8, 9]
]


Output:  
[
[7, 4, 1],
[8, 5, 2],
[9, 6, 3]
]


**Constraints:**  
- n == matrix.length == matrix[i].length  
- 1 <= n <= 20  
- -1000 <= matrix[i][j] <= 1000

---

## Approach  
- Perform the rotation layer by layer.  
- For each cell in the current layer, perform a 4-way swap with the corresponding elements from the other three sides.  
- Iterate over half the rows and half the columns.  
- Swap the elements in place without extra memory.

---

## Time and Space Complexity  
- **Time Complexity:** O(n²), where n is the dimension of the matrix.  
- **Space Complexity:** O(1), since the rotation is done in place.

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for(int i = 0; i < (n + 1) / 2; i++) {
            for(int j = 0; j < n / 2; j++) {
                int temp = matrix[n - 1 - j][i];

                matrix[n - 1 - j][i] = matrix[n - 1 - i][n - 1 - j];
                matrix[n - 1 - i][n - 1 - j] = matrix[j][n - 1 - i];
                matrix[j][n - 1 - i] = matrix[i][j];
                matrix[i][j] = temp;
            }
        }
    }
}
```
