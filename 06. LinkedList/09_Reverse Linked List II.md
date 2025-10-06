# 92. Reverse Linked List II
**Medium**

## Problem Statement
Reverse a linked list from position m to n. Do it in one-pass.

Note: 1 ≤ m ≤ n ≤ length of list.

## Examples
### Example 1:
- Input: head = [1,2,3,4,5], m = 2, n = 4
- Output: [1,4,3,2,5]

### Example 2:
- Input: head = [5], m = 1, n = 1
- Output: [5]

## Constraints
- The number of nodes in the list is in the range [1, 500].
- -500 <= Node.val <= 500
- 1 <= m <= n <= length of list

## Solution
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
    public ListNode reverseBetween(ListNode head, int m, int n) {
        if (head == null || m == n) return head;
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prev = dummy;
        // Step 1: move prev to node before m
        for (int i = 1; i < m; i++) {
            prev = prev.next;
        }
        // Step 2: reverse sublist from m to n
        ListNode current = prev.next;
        ListNode next = null;
        ListNode tail = current; // tail of reversed sublist
        for (int i = 0; i <= n - m; i++) {
            ListNode temp = current.next;
            current.next = next;
            next = current;
            current = temp;
        }
        // Step 3: reconnect
        prev.next = next;
        tail.next = current;
        return dummy.next;
    }
}
```

Approach
- Use a dummy node to handle edge cases easily.
- Move a pointer to the node just before position m.
- Reverse the sublist from m to n using standard reversal logic.
- Reconnect the reversed sublist back to the original list.
- Return the new head (dummy.next).

Time Complexity
O(n), where n is the length of the list.

Space Complexity
O(1), constant extra space.
