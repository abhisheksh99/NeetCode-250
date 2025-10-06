# 124. Binary Tree Maximum Path Sum
**Hard**

## Problem Statement
A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The path sum of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the maximum path sum of any non-empty path.

## Examples
### Example 1:
- Input: root = [1,2,3]
- Output: 6
- Explanation: The optimal path is 2 -> 1 -> 3.

### Example 2:
- Input: root = [-10,9,20,null,null,15,7]
- Output: 42
- Explanation: The optimal path is 15 -> 20 -> 7.

## Constraints
- The number of nodes in the tree is in the range [1, 3 * 10^4].
- -1000 <= Node.val <= 1000

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
    private int maxSum = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        dfs(root);
        return maxSum;
    }
    private int dfs(TreeNode node) {
        if (node == null) return 0;
        // compute max path sum from left/right children, ignore negatives
        int left = Math.max(0, dfs(node.left));
        int right = Math.max(0, dfs(node.right));
        // update global max if path through this node is better
        maxSum = Math.max(maxSum, left + right + node.val);
        // return max path sum extending from this node (to parent)
        return node.val + Math.max(left, right);
    }
}
```

Approach
- Use postorder DFS to compute the maximum path sum for each node.
- For each node, calculate the maximum path sum from left and right children (ignore negative sums).
- Update the global maximum if the path through the current node is better.
- Return the maximum path sum extending from the current node to its parent.

Time Complexity
- O(n), where n is the number of nodes in the tree.

Space Complexity
- O(h), where h is the height of the tree (due to recursion stack).
