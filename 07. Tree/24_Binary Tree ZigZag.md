# 103. Binary Tree Zigzag Level Order Traversal
**Medium**

## Problem Statement
Given the root of a binary tree, return the zigzag level order traversal of its nodes' values. (i.e., from left to right, then right to left for the next level and alternate between).

## Examples
### Example 1:
- Input: root = [3,9,20,null,null,15,7]
- Output: [[3],[20,9],[15,7]]

### Example 2:
- Input: root = [1]
- Output: [[1]]

### Example 3:
- Input: root = []
- Output: []

## Constraints
- The number of nodes in the tree is in the range [0, 2000].
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
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) return result;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        boolean leftToRight = true;
        while (!queue.isEmpty()) {
            int size = queue.size();
            LinkedList<Integer> level = new LinkedList<>();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                if (leftToRight) {
                    level.addLast(node.val);
                } else {
                    level.addFirst(node.val);
                }
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
            result.add(level);
            leftToRight = !leftToRight;
        }
        return result;
    }
}
```

Approach
- Use a queue for level order traversal (BFS).
- Use a boolean flag to alternate the direction for each level.
- For left-to-right, add node values to the end of the list; for right-to-left, add to the front.
- After each level, flip the direction flag.

Time Complexity
- O(n), where n is the number of nodes in the tree.

Space Complexity
- O(n), for the queue and result storage.
