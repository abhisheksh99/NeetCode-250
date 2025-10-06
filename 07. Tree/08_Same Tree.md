# 100. Same Tree
**Easy**

## Problem Statement
Given the roots of two binary trees p and q, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

## Examples
### Example 1:
- Input: p = [1,2,3], q = [1,2,3]
- Output: true

### Example 2:
- Input: p = [1,2], q = [1,null,2]
- Output: false

### Example 3:
- Input: p = [1,2,1], q = [1,1,2]
- Output: false

## Constraints
- The number of nodes in both trees is in the range [0, 100].
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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        }
        if (p == null || q == null) {
            return false;
        }
        if (p.val != q.val) {
            return false;
        }
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
}
```

Approach
- Use recursion to compare both trees node by node.
- If both nodes are null, return true.
- If only one is null or values differ, return false.
- Recursively check left and right subtrees.

Time Complexity
- O(n), where n is the number of nodes in the smaller tree.

Space Complexity
- O(h), where h is the height of the tree (due to recursion stack).
