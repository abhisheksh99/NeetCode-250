# 145. Binary Tree Postorder Traversal
**Easy**

## Problem Statement
Given the root of a binary tree, return the postorder traversal of its nodes' values.

## Examples
### Example 1:
- Input: root = [1,null,2,3]
- Output: [3,2,1]

### Example 2:
- Input: root = []
- Output: []

### Example 3:
- Input: root = [1]
- Output: [1]

## Constraints
- The number of nodes in the tree is in the range [0, 100].
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
public class Solution {
    private List<Integer> res;
    public List<Integer> postorderTraversal(TreeNode root) {
        res = new ArrayList<>();
        postorder(root);
        return res;
    }
    private void postorder(TreeNode node) {
        if (node == null) {
            return;
        }
        postorder(node.left);
        postorder(node.right);
        res.add(node.val);
    }
}
```

Approach
- Use recursion to traverse the tree in postorder (left, right, root).
- Add each node's value to the result list in the correct order.
- Return the result list after traversal.

Time Complexity
- O(n), where n is the number of nodes in the tree.

Space Complexity
- O(n), for the result list and recursion stack in the worst case.
