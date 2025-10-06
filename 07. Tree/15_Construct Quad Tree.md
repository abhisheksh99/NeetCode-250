# 427. Construct Quad Tree
**Medium**

## Problem Statement
Given a n * n matrix grid of 0's and 1's only, return the root of a quad tree representing the grid.

Notice that you can assign the value of a node to True or False when isLeaf is False, and either is fine.

A quad tree is a tree data structure in which each internal node has exactly four children. Each node has a boolean value val and a boolean isLeaf. The val is true if the node represents a grid of 1's, and false otherwise. The isLeaf is true if the node is a leaf node, and false otherwise. The four children are topLeft, topRight, bottomLeft, and bottomRight.

## Examples
### Example 1:
- Input: grid = [[0,1],[1,0]]
- Output: Node(val = true, isLeaf = false, ...)

### Example 2:
- Input: grid = [[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,1,1,1,1],[1,1,1,1,1,1,1,1],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0]]
- Output: Node(val = true, isLeaf = false, ...)

## Constraints
- n == grid.length == grid[i].length
- n == 2^x where 0 <= x <= 6

## Solution
```java
/*
// Definition for a QuadTree node.
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;
    public Node() {
        this.val = false;
        this.isLeaf = false;
        this.topLeft = null;
        this.topRight = null;
        this.bottomLeft = null;
        this.bottomRight = null;
    }
    public Node(boolean val, boolean isLeaf) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = null;
        this.topRight = null;
        this.bottomLeft = null;
        this.bottomRight = null;
    }
    public Node(boolean val, boolean isLeaf, Node topLeft, Node topRight, Node bottomLeft, Node bottomRight) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = topLeft;
        this.topRight = topRight;
        this.bottomLeft = bottomLeft;
        this.bottomRight = bottomRight;
    }
}
*/
class Solution {
    public Node construct(int[][] grid) {
        int n = grid.length;
        return constructDFS(grid, 0, 0, n);
    }
    private Node constructDFS(int[][] grid, int row, int col, int length) {
        if (length == 1) {
            return new Node(grid[row][col] == 1, true);
        }
        int half = length / 2;
        Node topLeft = constructDFS(grid, row, col, half);
        Node topRight = constructDFS(grid, row, col + half, half);
        Node bottomLeft = constructDFS(grid, row + half, col, half);
        Node bottomRight = constructDFS(grid, row + half, col + half, half);
        // If all four children are leaves with the same value, merge into one leaf node
        if (topLeft.isLeaf && topRight.isLeaf && bottomLeft.isLeaf && bottomRight.isLeaf &&
            topLeft.val == topRight.val && topLeft.val == bottomLeft.val && topLeft.val == bottomRight.val) {
            return new Node(topLeft.val, true);
        }
        // Otherwise, current node is not a leaf
        return new Node(true, false, topLeft, topRight, bottomLeft, bottomRight);
    }
}
```

Approach
- Use recursion (DFS) to divide the grid into four quadrants.
- If all values in a quadrant are the same, create a leaf node.
- Otherwise, recursively construct child nodes for each quadrant.
- Merge nodes if all four children are leaves with the same value.

Time Complexity
- O(n^2), where n is the size of the grid.

Space Complexity
- O(n^2), for the recursion stack and tree nodes.
