# 203. Remove Linked List Elements

## Problem Statement
Given the `head` of a linked list and an integer `val`, remove all the nodes of the linked list that have `Node.val == val`, and return the new head.

## Examples

### Example 1:
```
Input: head = [1,2,6,3,4,5,6], val = 6
Output: [1,2,3,4,5]
```

### Example 2:
```
Input: head = [], val = 1
Output: []
```

### Example 3:
```
Input: head = [7,7,7,7], val = 7
Output: []
```

## Approach
We can solve this problem using a dummy node and a pointer to traverse the list:
1. Create a dummy node that points to the head of the list to handle edge cases where the head needs to be removed.
2. Use a pointer `current` to traverse the list, starting from the dummy node.
3. For each node, check if the next node's value equals the target value.
4. If it does, skip the next node by setting `current.next = current.next.next`.
5. If it doesn't, move the `current` pointer forward.
6. Return `dummy.next` as the new head of the modified list.

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
    public ListNode removeElements(ListNode head, int val) {
        // Create a dummy node that points to the head
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode current = dummy;
        
        while (current.next != null) {
            if (current.next.val == val) {
                // Skip the next node
                current.next = current.next.next;
            } else {
                // Move to the next node
                current = current.next;
            }
        }
        
        return dummy.next;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n) - We traverse the list once.
- **Space Complexity**: O(1) - We use a constant amount of extra space.

## Key Insights
1. **Dummy Node**: Using a dummy node simplifies handling the case where the head needs to be removed.
2. **In-place Modification**: We modify the list in place without using extra space.
3. **Single Pass**: The solution requires only a single pass through the list.
4. **Edge Case Handling**: The solution handles edge cases such as an empty list or a list where all nodes need to be removed.

## Edge Cases
1. Empty list (should return null)
2. List where all nodes have the target value
3. List where the head needs to be removed
4. List where the tail needs to be removed
5. List with alternating nodes to be removed and kept

## Follow-up
How would you modify the solution to remove all occurrences of the target value using recursion instead of iteration?