# 450. Delete Node in a BST
**Medium**

## Problem Statement
Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node (possibly updated) of the BST.

A node to be deleted has at most two children. The deletion must maintain the BST property.

## Examples
### Example 1:
- Input: root = [5,3,6,2,4,null,7], key = 3
- Output: [5,4,6,2,null,null,7]

### Example 2:
- Input: root = [5,3,6,2,4,null,7], key = 0
- Output: [5,3,6,2,4,null,7]

### Example 3:
- Input: root = [], key = 0
- Output: []

## Constraints
- The number of nodes in the tree is in the range [0, 10^4].
- -10^5 <= Node.val <= 10^5
- Each node has a unique value.
- The tree is a valid BST.

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
public class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return root;
        if (key > root.val) {
            root.right = deleteNode(root.right, key);
        } else if (key < root.val) {
            root.left = deleteNode(root.left, key);
        } else {
            if (root.left == null) return root.right;
            if (root.right == null) return root.left;
            TreeNode cur = root.right;
            while (cur.left != null) {
                cur = cur.left;
            }
            cur.left = root.left;
            TreeNode res = root.right;
            root = null;
            return res;
        }
        return root;
    }
}
```

Approach
- Use recursion to find the node to delete.
- If the node has one or no children, return the non-null child or null.
- If the node has two children, find the minimum node in the right subtree, replace the node's value, and delete the minimum node.
- Return the updated root.

Time Complexity
- O(h), where h is the height of the tree.

Space Complexity
- O(h), due to recursion stack.
