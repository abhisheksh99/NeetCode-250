# 111. Minimum Depth of Binary Tree

## Problem Statement
Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

### Examples

#### Example 1:
```
Input: root = [3,9,20,null,null,15,7]
Output: 2
```

#### Example 2:
```
Input: root = [2,null,3,null,4,null,5,null,6]
Output: 5
```

## Approach
This problem can be solved using either a breadth-first search (BFS) or a depth-first search (DFS) approach. The BFS approach is more efficient for finding the minimum depth as it explores all nodes at the current depth before moving on to nodes at the next depth level.

1. **Breadth-First Search (BFS)**:
   - Use a queue to keep track of nodes at the current depth level.
   - For each level, check if any node is a leaf node (both left and right children are null).
   - The first leaf node found will give the minimum depth.
   - If no leaf node is found at the current level, increment the depth and process the next level.

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
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int depth = 1;
        
        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            
            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll();
                
                // Check if this is a leaf node
                if (currentNode.left == null && currentNode.right == null) {
                    return depth;
                }
                
                if (currentNode.left != null) {
                    queue.offer(currentNode.left);
                }
                if (currentNode.right != null) {
                    queue.offer(currentNode.right);
                }
            }
            
            depth++;
        }
        
        return depth;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n), where n is the number of nodes in the tree. In the worst case, we might have to visit all nodes.
- **Space Complexity**: O(n), as we need to store all nodes at the current depth level in the queue. In the worst case, this would be O(n/2) for a complete binary tree, which simplifies to O(n).

## Key Insights
1. **BFS for Minimum Depth**: BFS is more efficient than DFS for this problem because it stops as soon as it finds the first leaf node, which gives the minimum depth.
2. **Leaf Node Check**: The key is to identify leaf nodes, which are nodes with no children.
3. **Level Order Traversal**: The BFS approach processes nodes level by level, making it easy to track the current depth.

## Edge Cases
1. **Empty Tree**: Return 0 if the tree is empty.
2. **Single Node**: Return 1 if the tree has only one node.
3. **Skewed Tree**: For a tree that is essentially a linked list (e.g., all left children or all right children), the minimum depth is the number of nodes.
4. **Full Binary Tree**: For a full binary tree, the minimum depth is the height of the tree, which is log₂(n+1) where n is the number of nodes.

## Follow-up
How would you modify the solution to find the maximum depth of the binary tree? (This would involve a simple modification to the BFS or using a recursive DFS approach.)