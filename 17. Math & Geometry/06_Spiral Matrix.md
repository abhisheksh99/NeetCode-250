# Spiral Matrix

**Difficulty:** Medium

## Problem Statement  
Given an `m x n` matrix, return all elements of the matrix in spiral order.

**Example:**  
Input:  
[
[1, 2, 3],
[4, 5, 6],
[7, 8, 9]
]


Output:  
[1, 2, 3, 6, 9, 8, 7, 4, 5]



**Constraints:**  
- `m == matrix.length`  
- `n == matrix[i].length`  
- 1 <= m, n <= 10  
- -100 <= matrix[i][j] <= 100

---

## Approach  
- Use a directions array to represent movement order: right, down, left, up.  
- Keep track of steps to take in horizontal and vertical directions, reducing the steps after each traversal.  
- Move in the current direction the required number of steps, collecting elements.  
- Rotate directions cyclically until no steps remain.

---

## Time and Space Complexity  
- **Time Complexity:** O(m * n), where m and n are the matrix dimensions.  
- **Space Complexity:** O(m * n) for the output list.

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<>();
        int[][] directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        int[] steps = {matrix[0].length, matrix.length - 1};

        int r = 0, c = -1, d = 0;
        while (steps[d % 2] > 0) {
            for (int i = 0; i < steps[d % 2]; i++) {
                r += directions[d][0];
                c += directions[d][1];
                res.add(matrix[r][c]);
            }
            steps[d % 2]--;
            d = (d + 1) % 4;
        }
        return res;
    }
}
```