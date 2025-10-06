# 74. Search a 2D Matrix
**Medium**

## Problem Statement
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:
- Integers in each row are sorted from left to right.
- The first integer of each row is greater than the last integer of the previous row.

## Examples
### Example 1:
- Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
- Output: true

### Example 2:
- Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
- Output: false

## Constraints
- m == matrix.length
- n == matrix[i].length
- 1 <= m, n <= 100
- -10^4 <= matrix[i][j], target <= 10^4

## Solution
```java
class Solution {
	public boolean searchMatrix(int[][] matrix, int target) {
		int m=matrix.length;
		int n=matrix[0].length;
		int left=0,right=m*n-1;

		while(left<=right){
			int mid=left+(right-left)/2;
			int mid_val=matrix[mid/n][mid%n];
			if(mid_val==target){
				return true;
			}else if(mid_val<target){
				left=mid+1;
			}else{
				right=mid-1;
			}
		}
		return false;
	}
}
```

Approach

- Treat the 2D matrix as a 1D sorted array.
- Use binary search on the range [0, m*n-1].
- Map the 1D index to 2D coordinates: row = mid / n, col = mid % n.
- Compare the value at (row, col) to the target and adjust the search bounds accordingly.
- Return true if found, false otherwise.

Time Complexity

O(log(m*n)), where m is the number of rows and n is the number of columns.

Space Complexity

O(1), as the search is performed in-place.
