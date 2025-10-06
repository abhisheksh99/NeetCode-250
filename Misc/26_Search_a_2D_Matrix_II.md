# 240. Search a 2D Matrix II

## Problem Statement
Write an efficient algorithm that searches for a value `target` in an `m x n` integer matrix `matrix`. This matrix has the following properties:

- Integers in each row are sorted in ascending from left to right.
- Integers in each column are sorted in ascending from top to bottom.

### Examples

#### Example 1:
```
Input: matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
Output: true
```

#### Example 2:
```
Input: matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
Output: false
```

## Approach
This problem can be efficiently solved using a search space reduction technique. The key insight is to start from the top-right corner (or bottom-left corner) of the matrix and eliminate either a row or a column in each step based on the comparison with the target value.

1. **Search Space Reduction**:
   - Start from the top-right corner of the matrix.
   - If the current element is equal to the target, return `true`.
   - If the current element is greater than the target, move left (to a smaller value in the same row).
   - If the current element is less than the target, move down (to a larger value in the next row).
   - If we go out of bounds, the target is not present in the matrix.

### Solution Code
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        
        int row = 0;
        int col = matrix[0].length - 1; // Start from the top-right corner
        
        while (row < matrix.length && col >= 0) {
            if (matrix[row][col] == target) {
                return true;
            } else if (matrix[row][col] > target) {
                // Move left in the current row
                col--;
            } else {
                // Move down to the next row
                row++;
            }
        }
        
        return false;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(m + n), where m is the number of rows and n is the number of columns in the matrix. In the worst case, we might traverse one full row and one full column.
- **Space Complexity**: O(1), as we use a constant amount of extra space.

## Key Insights
1. **Search Space Reduction**: By starting from the top-right corner, we can eliminate either a row or a column in each step, reducing the search space efficiently.
2. **Matrix Properties**: The sorted order of rows and columns is crucial for this approach to work. It allows us to make informed decisions about which direction to move in.
3. **Efficiency**: The algorithm is optimal for this problem, as it avoids the need for binary search in each row or column, which would result in a higher time complexity.

## Edge Cases
1. **Empty Matrix**: If the matrix is empty, return `false`.
2. **Single Element**: If the matrix has only one element, check if it is equal to the target.
3. **Target Not Found**: If the target is not present in the matrix, return `false`.
4. **Target at Boundaries**: The target could be at the first or last position of the matrix.

## Follow-up
How would you modify the solution if the matrix is very large and doesn't fit into memory? (This would involve techniques like external sorting or divide and conquer.)