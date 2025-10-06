# 107. Binary Tree Level Order Traversal II

## Problem Statement
Given the `root` of a binary tree, return the bottom-up level order traversal of its nodes' values. (i.e., from left to right, level by level from leaf to root).

### Examples

#### Example 1:
```
Input: root = [3,9,20,null,null,15,7]
Output: [[15,7],[9,20],[3]]
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
This problem can be solved using a breadth-first search (BFS) approach with a queue. The key insight is to perform a standard level order traversal and then reverse the result at the end to get the bottom-up order.

1. **Edge Case Handling**: If the root is null, return an empty list.
2. **BFS Initialization**: Use a queue to perform BFS starting from the root node.
3. **Level-wise Processing**: For each level, process all nodes at that level, collect their values, and add their children to the queue for the next level.
4. **Store Levels**: Store each level's values in a temporary list and add it to the result list.
5. **Reverse the Result**: After processing all levels, reverse the result list to get the bottom-up order.

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
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            List<Integer> currentLevel = new ArrayList<>();
            
            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll();
                currentLevel.add(currentNode.val);
                
                if (currentNode.left != null) {
                    queue.offer(currentNode.left);
                }
                if (currentNode.right != null) {
                    queue.offer(currentNode.right);
                }
            }
            
            // Add the current level at the beginning to avoid reversing later
            result.add(0, currentLevel);
        }
        
        return result;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n), where n is the number of nodes in the tree. Each node is processed exactly once.
- **Space Complexity**: O(n), which is the space required for the queue and the result list. In the worst case, the queue can contain up to n/2 nodes (the last level of a complete binary tree).

## Key Insights
1. **BFS for Level Order Traversal**: BFS is the natural choice for level order traversal as it processes nodes level by level.
2. **Efficient Reversal**: By adding each level's list to the beginning of the result list, we avoid the need for an explicit reversal step at the end, improving performance.
3. **Queue for Level Tracking**: The queue ensures that nodes are processed level by level, and the level size is tracked to separate different levels.

## Edge Cases
1. **Empty Tree**: Return an empty list.
2. **Single Node Tree**: Return a list containing a single list with the node's value.
3. **Skewed Tree**: The solution should work correctly for left-skewed and right-skewed trees.
4. **Balanced Tree**: The solution should handle balanced trees efficiently.

## Follow-up
How would you modify the solution to perform a top-down level order traversal (from root to leaves) instead of bottom-up?