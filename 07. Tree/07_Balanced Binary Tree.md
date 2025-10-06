# 110. Balanced Binary Tree
**Easy**

## Problem Statement
Given a binary tree, determine if it is height-balanced.

A height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differs by more than one.

## Examples
### Example 1:
- Input: root = [3,9,20,null,null,15,7]
- Output: true

### Example 2:
- Input: root = [1,2,2,3,3,null,null,4,4]
- Output: false

### Example 3:
- Input: root = []
- Output: true

## Constraints
- The number of nodes in the tree is in the range [0, 5000].
- -10^4 <= Node.val <= 10^4

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
    public boolean isBalanced(TreeNode root) {
        if (root == null) {
            return true;
        }
        int leftHeight = getHeight(root.left);
        int rightHeight = getHeight(root.right);
        if (Math.abs(leftHeight - rightHeight) > 1) {
            return false;
        }
        return isBalanced(root.left) && isBalanced(root.right);
    }
    private int getHeight(TreeNode node) {
        if (node == null) {
            return 0;
        }
        int leftHeight = getHeight(node.left);
        int rightHeight = getHeight(node.right);
        return Math.max(leftHeight, rightHeight) + 1;
    }
}
```

Approach
- Use recursion to check the height of left and right subtrees for every node.
- If the height difference is more than 1 at any node, return false.
- Otherwise, recursively check for all nodes.

Time Complexity
- O(n^2) in the worst case (due to repeated height calculations).

Space Complexity
- O(h), where h is the height of the tree (due to recursion stack).
