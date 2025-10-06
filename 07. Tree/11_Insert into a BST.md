# 701. Insert into a Binary Search Tree
**Medium**

## Problem Statement
You are given the root node of a binary search tree (BST) and a value to insert into the tree. Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST.

Notice that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.

## Examples
### Example 1:
- Input: root = [4,2,7,1,3], val = 5
- Output: [4,2,7,1,3,5]

### Example 2:
- Input: root = [40,20,60,10,30,50,70], val = 25
- Output: [40,20,60,10,30,50,70,null,null,25]

### Example 3:
- Input: root = [4,2,7,1,3,null,null,null,null,null,null], val = 5
- Output: [4,2,7,1,3,5]

## Constraints
- The number of nodes in the tree will be in the range [0, 10^4].
- -10^8 <= Node.val <= 10^8
- All the values Node.val are unique.
- -10^8 <= val <= 10^8
- It's guaranteed that val does not exist in the original BST.

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
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) {
            return new TreeNode(val);
        }
        if (val > root.val) {
            root.right = insertIntoBST(root.right, val);
        } else {
            root.left = insertIntoBST(root.left, val);
        }
        return root;
    }
}
```

Approach
- Use recursion to find the correct position for the new value.
- If the current node is null, insert the new value here.
- If the value is greater than the current node, insert into the right subtree; otherwise, insert into the left subtree.
- Return the root after insertion.

Time Complexity
- O(h), where h is the height of the tree.

Space Complexity
- O(h), due to recursion stack.
