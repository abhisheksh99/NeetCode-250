# 24. Swap Nodes in Pairs

## Problem Statement
Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

## Examples

### Example 1:
```
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```

### Example 2:
```
Input: head = []
Output: []
```

### Example 3:
```
Input: head = [1]
Output: [1]
```

## Approach
We can solve this problem using an iterative approach with a dummy node to handle edge cases:
1. Create a dummy node that points to the head of the list.
2. Use a pointer `prev` to keep track of the node before the current pair.
3. While there are at least two nodes left to swap:
   - Identify the two nodes to be swapped: `first` and `second`.
   - Update the `next` pointers to swap the nodes.
   - Move the `prev` pointer to the node before the next pair.
4. Return the new head of the modified list.

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
    public ListNode swapPairs(ListNode head) {
        // Dummy node to handle edge cases
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prev = dummy;
        
        while (head != null && head.next != null) {
            // Nodes to be swapped
            ListNode first = head;
            ListNode second = head.next;
            
            // Swapping
            prev.next = second;
            first.next = second.next;
            second.next = first;
            
            // Move pointers for the next pair
            prev = first;
            head = first.next;
        }
        
        return dummy.next;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n) - We traverse the list once, where n is the number of nodes.
- **Space Complexity**: O(1) - We use a constant amount of extra space.

## Key Insights
1. **Dummy Node**: Using a dummy node simplifies handling edge cases, such as when the list is empty or has only one node.
2. **In-place Swapping**: We swap nodes by updating the `next` pointers without modifying the node values.
3. **Pointer Management**: The `prev` pointer helps maintain the correct links between the swapped pairs and the rest of the list.

## Edge Cases
1. Empty list (should return null)
2. List with a single node (should return the same list)
3. List with an odd number of nodes (the last node remains as is)
4. List with an even number of nodes (all nodes are swapped in pairs)

## Follow-up
How would you modify the solution to swap nodes in groups of k instead of pairs?