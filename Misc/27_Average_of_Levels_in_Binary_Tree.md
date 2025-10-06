# 637. Average of Levels in Binary Tree

## Problem Statement
Given the `root` of a binary tree, return the average value of the nodes on each level in the form of an array. Answers within `10-5` of the actual answer will be accepted.

### Examples

#### Example 1:
```
Input: root = [3,9,20,null,null,15,7]
Output: [3.00000,14.50000,11.00000]
Explanation: The average value of nodes on level 0 is 3, on level 1 is 14.5, and on level 2 is 11.
Hence return [3, 14.5, 11].
```

#### Example 2:
```
Input: root = [3,9,20,15,7]
Output: [3.00000,14.50000,11.00000]
```

## Approach
This problem can be efficiently solved using a breadth-first search (BFS) approach to traverse the tree level by level. The key idea is to process each level, calculate the average of the node values at that level, and add it to the result list.

1. **Breadth-First Search (BFS)**:
   - Use a queue to keep track of nodes at the current level.
   - For each level, calculate the sum of node values and count the number of nodes.
   - Compute the average and add it to the result list.
   - Enqueue the children of the current level's nodes for the next level.
   - Repeat until all levels are processed.

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
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> result = new ArrayList<>();
        if (root == null) return result;
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            double levelSum = 0;
            
            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll();
                levelSum += currentNode.val;
                
                if (currentNode.left != null) {
                    queue.offer(currentNode.left);
                }
                if (currentNode.right != null) {
                    queue.offer(currentNode.right);
                }
            }
            
            double levelAverage = levelSum / levelSize;
            result.add(levelAverage);
        }
        
        return result;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n), where n is the number of nodes in the binary tree. Each node is processed exactly once.
- **Space Complexity**: O(m), where m is the maximum number of nodes at any level in the tree. In the worst case, the queue can hold up to n/2 nodes (the last level of a complete binary tree).

## Key Insights
1. **Level Order Traversal**: BFS is ideal for processing nodes level by level.
2. **Efficient Averaging**: By tracking the number of nodes at each level, we can compute the average in constant time per level.
3. **Queue Usage**: A queue helps in efficiently managing the nodes at each level and their children for the next level.

## Edge Cases
1. **Empty Tree**: Return an empty list if the tree is empty.
2. **Single Node**: Return a list with the single node's value.
3. **Skewed Tree**: The tree could be left-skewed or right-skewed, but the BFS approach still works efficiently.
4. **Large Tree**: The solution handles trees with a large number of nodes efficiently due to its O(n) time complexity.

## Follow-up
How would you modify the solution to find the median of values at each level instead of the average? (This would require sorting the values at each level, increasing the time complexity.)