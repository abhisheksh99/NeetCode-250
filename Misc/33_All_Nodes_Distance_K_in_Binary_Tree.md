# 863. All Nodes Distance K in Binary Tree

## Problem Statement
Given the `root` of a binary tree, the value of a target node `target`, and an integer `k`, return an array of the values of all nodes that are at distance `k` from the target node.

You can return the answer in any order.

### Examples

#### Example 1:
```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, k = 2
Output: [7,4,1]
Explanation: The nodes that are a distance 2 from the target node (with value 5) have values 7, 4, and 1.
```

#### Example 2:
```
Input: root = [1], target = 1, k = 3
Output: []
```

## Approach
This problem can be solved using a combination of depth-first search (DFS) and breadth-first search (BFS). The key insight is to treat the binary tree as an undirected graph and perform a BFS starting from the target node to find all nodes at distance `k`.

1. **Build Parent Map**: First, we perform a DFS to build a hash map that stores each node's parent. This allows us to traverse upwards from any node.
2. **BFS from Target**: Starting from the target node, perform a BFS to explore all nodes at distance `k`. Keep track of visited nodes to avoid cycles.
3. **Collect Results**: After completing the BFS, collect all nodes at the k-th level.

### Solution Code
```java
import java.util.*;

class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

class Solution {
    private Map<TreeNode, TreeNode> parentMap;
    
    public List<Integer> distanceK(TreeNode root, TreeNode target, int k) {
        parentMap = new HashMap<>();
        // Build the parent map using DFS
        buildParentMap(root, null);
        
        // Perform BFS starting from the target node
        Queue<TreeNode> queue = new LinkedList<>();
        Set<TreeNode> visited = new HashSet<>();
        queue.offer(target);
        visited.add(target);
        
        int distance = 0;
        while (!queue.isEmpty() && distance < k) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode current = queue.poll();
                
                // Add left child
                if (current.left != null && !visited.contains(current.left)) {
                    queue.offer(current.left);
                    visited.add(current.left);
                }
                
                // Add right child
                if (current.right != null && !visited.contains(current.right)) {
                    queue.offer(current.right);
                    visited.add(current.right);
                }
                
                // Add parent
                TreeNode parent = parentMap.get(current);
                if (parent != null && !visited.contains(parent)) {
                    queue.offer(parent);
                    visited.add(parent);
                }
            }
            distance++;
        }
        
        // Collect the result
        List<Integer> result = new ArrayList<>();
        while (!queue.isEmpty()) {
            result.add(queue.poll().val);
        }
        
        return result;
    }
    
    private void buildParentMap(TreeNode node, TreeNode parent) {
        if (node == null) {
            return;
        }
        parentMap.put(node, parent);
        buildParentMap(node.left, node);
        buildParentMap(node.right, node);
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n), where n is the number of nodes in the tree. We perform a DFS to build the parent map and a BFS to find nodes at distance `k`, both of which take O(n) time.
- **Space Complexity**: O(n), which is the space required for the parent map, queue, and visited set. In the worst case, the queue can contain all nodes at a certain level, which is O(n).

## Key Insights
1. **Parent Map**: The parent map allows us to traverse the tree in any direction, treating it as an undirected graph.
2. **BFS for Level Order Traversal**: BFS is used to explore nodes level by level, which is perfect for finding nodes at a specific distance.
3. **Visited Set**: The visited set prevents revisiting nodes and ensures we don't get stuck in cycles.

## Edge Cases
1. **Empty Tree**: Return an empty list.
2. **Single Node Tree**: If k > 0, return an empty list. If k = 0, return the node's value.
3. **Target at Root**: The solution should work correctly when the target is the root node.
4. **Large k**: If k exceeds the maximum possible distance in the tree, return an empty list.

## Follow-up
How would you modify the solution if the tree is a binary search tree (BST) instead of a general binary tree? Could you optimize the solution further in that case?