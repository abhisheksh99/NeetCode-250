# 1325. Delete Leaves With a Given Value
**Medium**

## Problem Statement
Given a binary tree root and an integer target, delete all the leaf nodes with value target.

Note that once you delete a leaf node with value target, if its parent becomes a leaf and has the value target, it should also be deleted (you need to continue doing that until you cannot).

## Examples
### Example 1:
- Input: root = [1,2,3,2,null,2,4], target = 2
- Output: [1,null,3,null,4]

### Example 2:
- Input: root = [1,3,3,3,2], target = 3
- Output: [1,3,null,null,2]

### Example 3:
- Input: root = [1,2,null,2,null,2], target = 2
- Output: [1]

## Constraints
- The number of nodes in the tree is in the range [1, 3000].
- 1 <= Node.val, target <= 1000

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
    public TreeNode removeLeafNodes(TreeNode root, int target) {
        if (root == null) {
            return null;
        }
        root.left = removeLeafNodes(root.left, target);
        root.right = removeLeafNodes(root.right, target);
        if (root.left == null && root.right == null && root.val == target) {
            return null;
        }
        return root;
    }
}
```

Approach
- Use postorder DFS to process children before the parent.
- Recursively remove target-valued leaves from left and right subtrees.
- If the current node becomes a leaf and its value is target, remove it as well.
- Return the updated root.

Time Complexity
- O(n), where n is the number of nodes in the tree.

Space Complexity
- O(h), where h is the height of the tree (due to recursion stack).
