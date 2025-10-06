# 235. Lowest Common Ancestor of a Binary Search Tree
**Medium**

## Problem Statement
Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

The lowest common ancestor is defined as the lowest node in the tree that has both p and q as descendants (where we allow a node to be a descendant of itself).

## Examples
### Example 1:
- Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
- Output: 6

### Example 2:
- Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
- Output: 2

### Example 3:
- Input: root = [2,1], p = 2, q = 1
- Output: 2

## Constraints
- The number of nodes in the tree is in the range [2, 10^5].
- -10^9 <= Node.val <= 10^9
- All Node.val are unique.
- p != q
- p and q will exist in the BST.

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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        TreeNode cur = root;
        while (cur != null) {
            if (p.val > cur.val && q.val > cur.val) {
                cur = cur.right;
            } else if (p.val < cur.val && q.val < cur.val) {
                cur = cur.left;
            } else {
                return cur;
            }
        }
        return null;
    }
}
```

Approach
- Use the properties of BST: left < root < right.
- Traverse the tree from the root.
- If both p and q are greater than current node, move right.
- If both are less, move left.
- Otherwise, current node is the LCA.

Time Complexity
- O(h), where h is the height of the tree.

Space Complexity
- O(1), iterative approach uses constant space.
