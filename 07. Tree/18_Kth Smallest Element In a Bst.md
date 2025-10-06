# 230. Kth Smallest Element in a BST
**Medium**

## Problem Statement
Given the root of a binary search tree, and an integer k, return the kth smallest value (1-indexed) of all the values of the nodes in the tree.

## Examples
### Example 1:
- Input: root = [3,1,4,null,2], k = 1
- Output: 1

### Example 2:
- Input: root = [5,3,6,2,4,null,null,1], k = 3
- Output: 3

## Constraints
- The number of nodes in the tree is n.
- 1 <= k <= n <= 10^4
- 0 <= Node.val <= 10^4

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
    public ArrayList<Integer> inOrder(TreeNode root, ArrayList<Integer> arr) {
        if (root == null) {
            return arr;
        }
        inOrder(root.left, arr);
        arr.add(root.val);
        inOrder(root.right, arr);
        return arr;
    }
    public int kthSmallest(TreeNode root, int k) {
        ArrayList<Integer> nums = inOrder(root, new ArrayList<Integer>());
        return nums.get(k - 1);
    }
}
```

Approach
- Use inorder traversal to collect all node values in sorted order.
- Return the (k-1)th element from the list (since list is 0-indexed).

Time Complexity
- O(n), where n is the number of nodes in the tree.

Space Complexity
- O(n), for the list storing all node values and recursion stack.
