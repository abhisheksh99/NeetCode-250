# 25. Reverse Nodes in k-Group
**Hard**

## Problem Statement
Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

## Examples
### Example 1:
- Input: head = [1,2,3,4,5], k = 2
- Output: [2,1,4,3,5]

### Example 2:
- Input: head = [1,2,3,4,5], k = 3
- Output: [3,2,1,4,5]

## Constraints
- The number of nodes in the list is in the range [1, 5000].
- 0 <= Node.val <= 1000
- 1 <= k <= 5000

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
    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null || k == 1) return head;
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prev = dummy, curr = head;
        // Count the number of nodes in the list
        int count = 0;
        while (curr != null) {
            count++;
            curr = curr.next;
        }
        // Reverse k nodes at a time
        while (count >= k) {
            curr = prev.next;
            ListNode next = curr.next;
            // Reverse k nodes
            for (int i = 1; i < k; i++) {
                curr.next = next.next;
                next.next = prev.next;
                prev.next = next;
                next = curr.next;
            }
            prev = curr;
            count -= k;
        }
        return dummy.next;
    }
}
```

Approach
- Use a dummy node to simplify edge cases.
- Count the total number of nodes in the list.
- For every group of k nodes, reverse the group in place.
- Move the prev pointer to the end of the reversed group and repeat until fewer than k nodes remain.
- Return the new head (dummy.next).

Time Complexity
- O(n), where n is the number of nodes in the list.

Space Complexity
- O(1), constant extra space.
