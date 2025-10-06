# Search in Binary Tree
**Easy**

## Problem Statement
Given the root of a binary tree and an integer value, return true if the value exists in the tree, otherwise return false.

## Examples
### Example 1:
- Input: root = [4,2,7,1,3], value = 2
- Output: true

### Example 2:
- Input: root = [4,2,7,1,3], value = 5
- Output: false

## Constraints
- The number of nodes in the tree is in the range [0, 10^4].
- -10^5 <= Node.val <= 10^5
- The tree is not necessarily a BST.

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
    public boolean search(TreeNode root, int value) {
        if (root == null) return false;
        if (root.val == value) return true;
        return search(root.left, value) || search(root.right, value);
    }
}
```

Approach
- Use DFS (recursive) to traverse the tree.
- At each node, check if the value matches.
- If not, search left and right subtrees.
- Return true if found in any subtree, else false.

Time Complexity
- O(n), where n is the number of nodes in the tree.

Space Complexity
- O(h), where h is the height of the tree (due to recursion stack).
