# 876. Middle of the Linked List

## Problem Statement
Given the `head` of a singly linked list, return the middle node of the linked list. If there are two middle nodes, return the second middle node.

## Examples

### Example 1:
```
Input: head = [1,2,3,4,5]
Output: [3,4,5]
Explanation: The middle node of the list is node 3.
```

### Example 2:
```
Input: head = [1,2,3,4,5,6]
Output: [4,5,6]
Explanation: Since the list has two middle nodes with values 3 and 4, we return the second one.
```

## Approach
We can solve this problem efficiently using the "tortoise and hare" (two-pointer) approach:
1. Use two pointers: `slow` and `fast`, both starting at the head of the list.
2. Move `slow` one step at a time, and `fast` two steps at a time.
3. When `fast` reaches the end of the list, `slow` will be at the middle node.
4. If the list has an even number of nodes, `slow` will be at the second middle node when `fast` reaches the end.

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
    public ListNode middleNode(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        return slow;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n) - We traverse the list once with the fast pointer.
- **Space Complexity**: O(1) - We use a constant amount of extra space.

## Key Insights
1. **Two-pointer Technique**: The "tortoise and hare" approach allows us to find the middle in a single pass.
2. **Even and Odd Length**: The solution handles both even and odd length lists correctly, returning the second middle node for even lengths.
3. **Efficiency**: This approach is optimal with O(n) time and O(1) space complexity.
4. **Termination Condition**: The loop continues as long as `fast` can move two steps ahead.

## Edge Cases
1. Empty list (should return null)
2. List with a single node (should return that node)
3. List with two nodes (should return the second node)
4. List with an odd number of nodes
5. List with an even number of nodes

## Follow-up
How would you modify the solution to return the first middle node when the list has an even number of nodes?