# 617. Merge Two Binary Trees

## Problem Statement
You are given two binary trees `root1` and `root2`.

Imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not. You need to merge the two trees into a new binary tree. The merge rule is that if two nodes overlap, then sum their node values as the new value of the merged node. Otherwise, the non-null node will be used as the node of the new tree.

Return the merged tree.

**Note:** The merging process must start from the root nodes of both trees.

### Examples

#### Example 1:
```
Input: root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
Output: [3,4,5,5,4,null,7]
```

#### Example 2:
```
Input: root1 = [1], root2 = [1,2]
Output: [2,2]
```

## Approach
This problem can be solved using a recursive depth-first search (DFS) approach. The idea is to traverse both trees simultaneously and create a new tree where each node's value is the sum of the corresponding nodes in the input trees. If one of the nodes is null, we use the value from the other tree.

1. **Base Case**: If both nodes are null, return null.
2. **Node Creation**: Create a new node with the sum of the values of the current nodes from both trees. If one node is null, treat its value as 0.
3. **Recursive Calls**: Recursively merge the left and right subtrees of both trees.
4. **Return the New Node**: The newly created node with its left and right children set to the results of the recursive calls.

### Solution Code
```java
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
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        // Base case: if both nodes are null, return null
        if (root1 == null && root2 == null) {
            return null;
        }
        
        // Get the values of the current nodes, or 0 if the node is null
        int val1 = (root1 != null) ? root1.val : 0;
        int val2 = (root2 != null) ? root2.val : 0;
        
        // Create a new node with the sum of the values
        TreeNode mergedNode = new TreeNode(val1 + val2);
        
        // Recursively merge the left and right subtrees
        mergedNode.left = mergeTrees(
            (root1 != null) ? root1.left : null,
            (root2 != null) ? root2.left : null
        );
        
        mergedNode.right = mergeTrees(
            (root1 != null) ? root1.right : null,
            (root2 != null) ? root2.right : null
        );
        
        return mergedNode;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n), where n is the number of nodes in the larger of the two trees. We visit each node exactly once.
- **Space Complexity**: O(h), where h is the height of the resulting tree. This is due to the recursion stack. In the worst case, the tree is skewed, and the height is O(n).

## Key Insights
1. **Recursive Approach**: The solution uses recursion to traverse both trees simultaneously, which simplifies the merging process.
2. **Handling Null Nodes**: The solution efficiently handles cases where one of the nodes is null by treating its value as 0 and continuing the traversal.
3. **In-Place Merging**: The solution can be modified to merge the trees in-place by reusing the nodes from one of the trees, which can save space.

## Edge Cases
1. **Both Trees Are Empty**: Return null.
2. **One Tree Is Empty**: Return the non-empty tree.
3. **Trees of Different Depths**: The resulting tree will have the structure of the deeper tree, with nodes from the shallower tree contributing their values where they exist.
4. **Single Node Trees**: The result should be a single node with the sum of the two nodes' values.

## Follow-up
How would you modify the solution to merge the trees in-place (i.e., without creating new nodes)?