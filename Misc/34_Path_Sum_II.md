# 113. Path Sum II

## Problem Statement
Given the `root` of a binary tree and an integer `targetSum`, return all root-to-leaf paths where the sum of the node values in the path equals `targetSum`. Each path should be returned as a list of the node values, not node references.

A root-to-leaf path is a path starting from the root and ending at any leaf node. A leaf is a node with no children.

### Examples

#### Example 1:
```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: [[5,4,11,2],[5,8,4,5]]
Explanation: There are two paths whose sum equals targetSum:
5 + 4 + 11 + 2 = 22
5 + 8 + 4 + 5 = 22
```

#### Example 2:
```
Input: root = [1,2,3], targetSum = 5
Output: []
```

#### Example 3:
```
Input: root = [1,2], targetSum = 0
Output: []
```

## Approach
This problem can be solved using a depth-first search (DFS) approach to explore all possible root-to-leaf paths. The key steps are:

1. **Recursive DFS Traversal**: Traverse the tree from the root to each leaf node.
2. **Path Tracking**: Maintain the current path from the root to the current node.
3. **Sum Calculation**: Keep track of the current sum of the path.
4. **Leaf Node Check**: When a leaf node is reached, check if the current sum equals the target sum. If so, add the current path to the result.
5. **Backtracking**: After exploring a path, backtrack by removing the current node from the path before returning to the parent node.

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
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> currentPath = new ArrayList<>();
        dfs(root, targetSum, currentPath, result);
        return result;
    }
    
    private void dfs(TreeNode node, int remainingSum, List<Integer> currentPath, List<List<Integer>> result) {
        if (node == null) {
            return;
        }
        
        // Add current node to the path
        currentPath.add(node.val);
        
        // Check if it's a leaf node and the sum matches the target
        if (node.left == null && node.right == null && remainingSum == node.val) {
            result.add(new ArrayList<>(currentPath));
        } else {
            // Recursively check left and right subtrees
            dfs(node.left, remainingSum - node.val, currentPath, result);
            dfs(node.right, remainingSum - node.val, currentPath, result);
        }
        
        // Backtrack: remove the current node from the path
        currentPath.remove(currentPath.size() - 1);
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n²), where n is the number of nodes in the tree. In the worst case, we might have to visit all nodes, and for each leaf node, we might have to copy the current path to the result, which takes O(n) time.
- **Space Complexity**: O(n), where n is the number of nodes in the tree. The space is used to store the recursion stack and the current path. In the worst case, the tree is a linked list, and the recursion stack will be of size O(n).

## Key Insights
1. **Backtracking**: The key to this problem is to use backtracking to explore all possible paths and backtrack once a path is fully explored.
2. **Leaf Node Check**: We only add a path to the result when we reach a leaf node and the sum of the path equals the target sum.
3. **Immutability**: We create a new list for each valid path to avoid modifying the current path list when adding to the result.

## Edge Cases
1. **Empty Tree**: Return an empty list.
2. **Single Node Tree**: If the node's value equals the target sum, return a list containing a single path with that node's value.
3. **No Valid Path**: If no path sums to the target, return an empty list.
4. **Negative Numbers**: The solution works with negative numbers in the tree and target sum.
5. **Large Tree**: The solution should handle the maximum constraint efficiently.

## Follow-up
How would you modify the solution if the path doesn't need to be from root to leaf, but can start and end at any node in the tree?