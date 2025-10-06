# 328. Odd Even Linked List

## Problem Statement
Given the `head` of a singly linked list, group all nodes with odd indices together followed by the nodes with even indices, and return the reordered list.

The first node is considered **odd**, and the second node is **even**, and so on.

Note that the relative order inside both the even and odd groups should remain as it was in the input.

## Examples

### Example 1:
```
Input: head = [1,2,3,4,5]
Output: [1,3,5,2,4]
```

### Example 2:
```
Input: head = [2,1,3,5,6,4,7]
Output: [2,3,6,7,1,5,4]
```

## Approach
We can solve this problem by separating the original linked list into two separate lists: one for odd-indexed nodes and another for even-indexed nodes. Then, we'll connect the end of the odd list to the head of the even list.

1. **Handle Edge Cases**: If the list is empty or has only one node, return the head as is.
2. **Initialize Pointers**: 
   - `odd` points to the first node (odd-indexed)
   - `even` points to the second node (even-indexed)
   - `evenHead` keeps track of the start of the even list
3. **Separate the Lists**: 
   - Connect all odd nodes together
   - Connect all even nodes together
4. **Combine the Lists**: Connect the end of the odd list to the head of the even list
5. **Return the Result**: The head of the modified list is the original head

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
    public ListNode oddEvenList(ListNode head) {
        // Edge cases: empty list or single node
        if (head == null || head.next == null) {
            return head;
        }
        
        ListNode odd = head;
        ListNode even = head.next;
        ListNode evenHead = even; // Save the start of even list
        
        while (even != null && even.next != null) {
            // Connect odd nodes
            odd.next = even.next;
            odd = odd.next;
            
            // Connect even nodes
            even.next = odd.next;
            even = even.next;
        }
        
        // Connect the end of odd list to the start of even list
        odd.next = evenHead;
        
        return head;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n) - We traverse the list once, where n is the number of nodes.
- **Space Complexity**: O(1) - We use a constant amount of extra space.

## Key Insights
1. **In-place Reordering**: We rearrange the nodes in place without using extra space for a new list.
2. **Two-pointer Technique**: We use two pointers (`odd` and `even`) to traverse and separate the list into two parts.
3. **Maintaining Order**: The relative order of nodes in both odd and even groups is preserved.

## Edge Cases
1. Empty list (should return null)
2. List with a single node (should return the same list)
3. List with two nodes (should remain unchanged)
4. List with an odd number of nodes
5. List with an even number of nodes

## Follow-up
How would you modify the solution if the problem was to group nodes with even values first, followed by nodes with odd values, while maintaining the relative order within each group?