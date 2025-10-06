# 1448. Count Good Nodes in Binary Tree
**Medium**

## Problem Statement
Given a binary tree root, a node X in the tree is named good if in the path from root to X there are no nodes with a value greater than X.

Return the number of good nodes in the binary tree.

## Examples
### Example 1:
- Input: root = [3,1,4,3,null,1,5]
- Output: 4
- Explanation: Nodes in blue are good. (3, 4, 5, 3)

### Example 2:
- Input: root = [3,3,null,4,2]
- Output: 3

### Example 3:
- Input: root = [1]
- Output: 1

## Constraints
- The number of nodes in the binary tree is in the range [1, 10^5].
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
    public int goodNodes(TreeNode root) {
        return countGoodNodes(root, Integer.MIN_VALUE);
    }
    private int countGoodNodes(TreeNode node, int maxSoFar) {
        if (node == null) {
            return 0;
        }
        int count = 0;
        if (node.val >= maxSoFar) {
            count = 1;
            maxSoFar = node.val;
        }
        count += countGoodNodes(node.left, maxSoFar);
        count += countGoodNodes(node.right, maxSoFar);
        return count;
    }
}
```

Approach
- Use DFS to traverse the tree.
- Pass the maximum value seen so far along the path.
- If the current node's value is greater than or equal to the max so far, count it as a good node.
- Recurse for left and right children, updating the max as needed.

Time Complexity
- O(n), where n is the number of nodes in the tree.

Space Complexity
- O(h), where h is the height of the tree (due to recursion stack).
