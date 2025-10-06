# 572. Subtree of Another Tree
**Easy**

## Problem Statement
Given the roots of two binary trees root and subRoot, return true if there is a subtree of root with the same structure and node values of subRoot and false otherwise.

A subtree of a binary tree tree is a tree that consists of a node in tree and all of this node's descendants. The tree tree could also be considered as a subtree of itself.

## Examples
### Example 1:
- Input: root = [3,4,5,1,2], subRoot = [4,1,2]
- Output: true

### Example 2:
- Input: root = [3,4,5,1,2,null,null,null,null,0], subRoot = [4,1,2]
- Output: false

## Constraints
- The number of nodes in both trees is in the range [1, 2000].
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
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        if (subRoot == null) {
            return true;
        }
        if (root == null) {
            return false;
        }
        if (sameTree(root, subRoot)) {
            return true;
        }
        return isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
    }
    public boolean sameTree(TreeNode root, TreeNode subRoot) {
        if (root == null && subRoot == null) {
            return true;
        }
        if (root != null && subRoot != null && root.val == subRoot.val) {
            return sameTree(root.left, subRoot.left) && sameTree(root.right, subRoot.right);
        }
        return false;
    }
}
```

Approach
- Use recursion to traverse the main tree.
- At each node, check if the subtree rooted at that node matches subRoot using a helper function.
- The helper function checks if two trees are identical.
- If a match is found, return true; otherwise, check left and right subtrees.

Time Complexity
- O(m * n), where m is the number of nodes in root and n is the number of nodes in subRoot.

Space Complexity
- O(h), where h is the height of the main tree (due to recursion stack).
