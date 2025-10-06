# 61. Rotate List

## Problem Statement
Given the `head` of a linked list, rotate the list to the right by `k` places.

## Examples

### Example 1:
```
Input: head = [1,2,3,4,5], k = 2
Output: [4,5,1,2,3]
```

### Example 2:
```
Input: head = [0,1,2], k = 4
Output: [2,0,1]
```

## Approach
To solve this problem, we can follow these steps:
1. **Handle Edge Cases**: If the list is empty, has only one node, or `k` is 0, return the head as is.
2. **Find the Length and Tail**: Traverse the list to find its length and locate the tail node.
3. **Calculate Effective Rotations**: Since rotating a list of length `n` by `n` times results in the original list, we can take `k % length` to find the effective number of rotations needed.
4. **Find the New Tail and Head**: Traverse the list again to find the new tail, which is the node at position `length - k % length - 1`. The new head will be the node following the new tail.
5. **Perform Rotation**: Update the `next` pointers to rotate the list.

## Solution Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        // Handle edge cases
        if (head == null || head.next == null || k == 0) {
            return head;
        }
        
        // Find the length of the list and the tail node
        int length = 1;
        ListNode tail = head;
        while (tail.next != null) {
            tail = tail.next;
            length++;
        }
        
        // Calculate the effective number of rotations
        k = k % length;
        if (k == 0) {
            return head; // No rotation needed
        }
        
        // Find the new tail (length - k - 1 steps from the start)
        ListNode newTail = head;
        for (int i = 0; i < length - k - 1; i++) {
            newTail = newTail.next;
        }
        
        // The new head is the node after the new tail
        ListNode newHead = newTail.next;
        
        // Break the list and connect the tail to the original head
        newTail.next = null;
        tail.next = head;
        
        return newHead;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n) - We traverse the list twice: once to find the length and once to find the new tail.
- **Space Complexity**: O(1) - We use a constant amount of extra space.

## Key Insights
1. **Effective Rotations**: By calculating `k % length`, we handle cases where `k` is larger than the length of the list.
2. **Two-Pass Approach**: The first pass determines the length and tail, while the second pass finds the new tail and head.
3. **In-place Rotation**: The rotation is done in-place by updating the `next` pointers of the relevant nodes.

## Edge Cases
1. Empty list (should return null)
2. List with a single node (should return the same list)
3. `k` is 0 (no rotation needed)
4. `k` is equal to the length of the list (no rotation needed)
5. `k` is larger than the length of the list (use modulo to find effective rotations)

## Follow-up
How would you modify the solution to rotate the list to the left by `k` places instead of to the right?