# 103. Binary Tree Zigzag Level Order Traversal

## Problem Statement
Given the `root` of a binary tree, return the zigzag level order traversal of its nodes' values. (i.e., from left to right, then right to left for the next level and alternate between).

### Examples

#### Example 1:
```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[20,9],[15,7]]
```

#### Example 2:
```
Input: root = [1]
Output: [[1]]
```

#### Example 3:
```
Input: root = []
Output: []
```

## Approach
This problem can be solved using a breadth-first search (BFS) approach with a queue. The key insight is to perform a standard level order traversal and reverse the order of nodes at every other level to achieve the zigzag pattern.

1. **Edge Case Handling**: If the root is null, return an empty list.
2. **BFS Initialization**: Use a queue to perform BFS starting from the root node.
3. **Level-wise Processing**: For each level, process all nodes at that level, collect their values, and add their children to the queue for the next level.
4. **Zigzag Ordering**: Use a boolean flag to determine whether to add nodes in normal or reverse order for each level.
5. **Store Levels**: Store each level's values in a temporary list and add it to the result list.

### Solution Code
```java
import java.util.*;

class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        boolean leftToRight = true;
        
        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            List<Integer> currentLevel = new ArrayList<>();
            
            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll();
                
                // Add the node to the current level based on the direction
                if (leftToRight) {
                    currentLevel.add(currentNode.val);
                } else {
                    currentLevel.add(0, currentNode.val);
                }
                
                // Add child nodes to the queue
                if (currentNode.left != null) {
                    queue.offer(currentNode.left);
                }
                if (currentNode.right != null) {
                    queue.offer(currentNode.right);
                }
            }
            
            // Add the current level to the result and toggle the direction
            result.add(currentLevel);
            leftToRight = !leftToRight;
        }
        
        return result;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n), where n is the number of nodes in the tree. Each node is processed exactly once.
- **Space Complexity**: O(n), which is the space required for the queue and the result list. In the worst case, the queue can contain up to n/2 nodes (the last level of a complete binary tree).

## Key Insights
1. **BFS for Level Order Traversal**: BFS is used to process nodes level by level, which is essential for this problem.
2. **Direction Toggling**: A boolean flag is used to alternate between left-to-right and right-to-left traversal for each level.
3. **Efficient Reversal**: Instead of reversing the list after processing each level, we add nodes to the front or back of the list based on the current direction, which is more efficient.
4. **Queue for Level Tracking**: The queue ensures that nodes are processed level by level, and the level size is tracked to separate different levels.

## Edge Cases
1. **Empty Tree**: Return an empty list.
2. **Single Node Tree**: Return a list containing a single list with the node's value.
3. **Skewed Tree**: The solution should work correctly for left-skewed and right-skewed trees.
4. **Balanced Tree**: The solution should handle balanced trees efficiently.

## Follow-up
How would you modify the solution to perform a spiral (zigzag) order traversal for an n-ary tree (a tree where each node can have more than two children)?