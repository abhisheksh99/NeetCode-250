# 304. Range Sum Query 2D - Immutable
**Medium**

## Problem Statement
Given a 2D matrix, handle multiple queries of the following type:
Calculate the sum of the elements of matrix inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

Implement the NumMatrix class:
- `NumMatrix(int[][] matrix)` Initializes the object with the integer matrix `matrix`.
- `int sumRegion(int row1, int col1, int row2, int col2)` Returns the sum of the elements of matrix inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

## Examples
### Example 1:
```
Input:
["NumMatrix","sumRegion","sumRegion","sumRegion"]
[[[[3,0,1,4,2],[5,6,3,2,1],[1,2,0,1,5],[4,1,0,1,7],[1,0,3,0,5]]],[2,1,4,3],[1,1,2,2],[1,2,2,4]]
Output:
[null, 8, 11, 12]
```

## Constraints
- m == matrix.length
- n == matrix[i].length
- 1 <= m, n <= 200
- -10^5 <= matrix[i][j] <= 10^5
- 0 <= row1 <= row2 < m
- 0 <= col1 <= col2 < n
- At most 10^4 calls will be made to sumRegion.

## Solution
```java
class NumMatrix {
	private int[][] prefixSum;

	public NumMatrix(int[][] matrix) {
		if (matrix.length == 0 || matrix[0].length == 0) return;

		int m = matrix.length;
		int n = matrix[0].length;
		prefixSum = new int[m + 1][n + 1]; // Extra row & col for easier calculation

		// Build prefix sum matrix
		for (int i = 1; i <= m; i++) {
			for (int j = 1; j <= n; j++) {
				prefixSum[i][j] = matrix[i-1][j-1] 
								+ prefixSum[i-1][j] 
								+ prefixSum[i][j-1] 
								- prefixSum[i-1][j-1];
			}
		}
	}
    
	public int sumRegion(int row1, int col1, int row2, int col2) {
		// Use inclusion-exclusion principle
		return prefixSum[row2+1][col2+1] 
			 - prefixSum[row1][col2+1] 
			 - prefixSum[row2+1][col1] 
			 + prefixSum[row1][col1];
	}
}
```

Approach

- Precompute a 2D prefix sum matrix with an extra row and column for easier calculations.
- For each cell (i, j), set `prefixSum[i][j]` to the sum of all elements in the submatrix from (0,0) to (i-1,j-1).
- To answer a sumRegion query, use the inclusion-exclusion principle:
  - sumRegion(row1, col1, row2, col2) = prefixSum[row2+1][col2+1] - prefixSum[row1][col2+1] - prefixSum[row2+1][col1] + prefixSum[row1][col1]
- Notes: This allows O(1) query time after O(mn) preprocessing.

Time Complexity

O(mn) for preprocessing, O(1) per query.

Space Complexity

O(mn), for the prefix sum matrix.
