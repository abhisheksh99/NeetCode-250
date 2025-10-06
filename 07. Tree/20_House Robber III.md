# 337. House Robber III
**Medium**

## Problem Statement
The thief has found himself a new place for his thievery again. There is only one entrance to this area, called root.

Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if two directly-linked houses were broken into on the same night.

Given the root of the binary tree, return the maximum amount of money the thief can rob without alerting the police.

## Examples
### Example 1:
- Input: root = [3,2,3,null,3,null,1]
- Output: 7
- Explanation: Maximum amount the thief can rob = 3 + 3 + 1 = 7.

### Example 2:
- Input: root = [3,4,5,1,3,null,1]
- Output: 9
- Explanation: Maximum amount the thief can rob = 4 + 5 = 9.

## Constraints
- The number of nodes in the tree is in the range [0, 10^4].
- 0 <= Node.val <= 10^4

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
    public int rob(TreeNode root) {
        int[] result = dfs(root);
        return Math.max(result[0], result[1]);
    }
    private int[] dfs(TreeNode root) {
        if (root == null) {
            return new int[]{0, 0};
        }
        int[] leftPair = dfs(root.left);
        int[] rightPair = dfs(root.right);
        int withRoot = root.val + leftPair[1] + rightPair[1];
        int withoutRoot = Math.max(leftPair[0], leftPair[1]) + Math.max(rightPair[0], rightPair[1]);
        return new int[]{withRoot, withoutRoot};
    }
}
```

Approach
- Use postorder DFS to compute two values for each node:
  - Maximum money if robbing this node (cannot rob children)
  - Maximum money if not robbing this node (can rob children)
- For each node, return an array: [rob this node, don't rob this node].
- The answer is the max of these two values at the root.

Time Complexity
- O(n), where n is the number of nodes in the tree.

Space Complexity
- O(h), where h is the height of the tree (due to recursion stack).
