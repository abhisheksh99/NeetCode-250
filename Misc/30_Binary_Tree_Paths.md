# 257. Binary Tree Paths

## Problem Statement
Given the `root` of a binary tree, return all root-to-leaf paths in any order.

A leaf is a node with no children.

### Examples

#### Example 1:
```
Input: root = [1,2,3,null,5]
Output: ["1->2->5","1->3"]
```

#### Example 2:
```
Input: root = [1]
Output: ["1"]
```

## Approach
This problem can be solved using a depth-first search (DFS) approach to traverse the binary tree and collect all root-to-leaf paths. The idea is to:

1. Start from the root node and traverse the tree while keeping track of the current path.
2. When a leaf node is encountered, add the current path to the result list.
3. For non-leaf nodes, recursively traverse the left and right subtrees, appending the current node's value to the path.
4. Use a helper function to perform the DFS and build the paths.

### Solution Code
```java
import java.util.ArrayList;
import java.util.List;

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
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        
        dfs(root, "", result);
        return result;
    }
    
    private void dfs(TreeNode node, String path, List<String> result) {
        // If we've reached a leaf node, add the current path to the result
        if (node.left == null && node.right == null) {
            result.add(path + node.val);
            return;
        }
        
        // Recursively traverse left and right subtrees
        if (node.left != null) {
            dfs(node.left, path + node.val + "->", result);
        }
        if (node.right != null) {
            dfs(node.right, path + node.val + "->", result);
        }
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity**: O(h), where h is the height of the tree. This is due to the recursion stack. In the worst case, the tree is completely unbalanced (e.g., a linked list), and the space complexity becomes O(n).

## Key Insights
1. **DFS Traversal**: The solution uses a depth-first search to explore all possible paths from the root to the leaves.
2. **Path Construction**: The current path is built incrementally as we traverse the tree, and it's added to the result list when a leaf node is reached.
3. **String Concatenation**: The solution uses string concatenation to build the path. While this is simple, it's worth noting that using a `StringBuilder` could be more efficient for very large trees.
4. **Base Case Handling**: The base case checks if the current node is a leaf node (both left and right children are null).

## Edge Cases
1. **Empty Tree**: If the input tree is empty, return an empty list.
2. **Single Node**: If the tree has only one node, return a list containing just that node's value as a string.
3. **Skewed Tree**: The solution should work correctly for left-skewed and right-skewed trees.
4. **Large Tree**: The solution should handle the maximum number of nodes efficiently.

## Follow-up
How would you modify the solution to return the paths as a list of lists of integers instead of strings? This would avoid string manipulation and might be more efficient for certain use cases.