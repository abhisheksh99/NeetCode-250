# 543. Diameter of Binary Tree
**Easy**

## Problem Statement
Given the root of a binary tree, return the length of the diameter of the tree.

The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

## Examples
### Example 1:
- Input: root = [1,2,3,4,5]
- Output: 3
- Explanation: The longest path is [4,2,1,3] or [5,2,1,3], and its length is 3.

### Example 2:
- Input: root = [1,2]
- Output: 1

## Constraints
- The number of nodes in the tree is in the range [1, 10^4].
- -100 <= Node.val <= 100

## Solution
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int maxDiameter = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        getHeight(root);
        return maxDiameter;
    }
    private int getHeight(TreeNode node) {
        if (node == null) return 0;
        int leftHeight = getHeight(node.left);
        int rightHeight = getHeight(node.right);
        maxDiameter = Math.max(maxDiameter, leftHeight + rightHeight);
        return 1 + Math.max(leftHeight, rightHeight);
    }
}
```

Approach
- Use recursion to compute the height of each subtree.
- At each node, update the maximum diameter as the sum of left and right subtree heights.
- The diameter is the maximum number of edges between any two nodes.
- Return the maximum diameter found.

Time Complexity
- O(n), where n is the number of nodes in the tree.

Space Complexity
- O(h), where h is the height of the tree (due to recursion stack).
