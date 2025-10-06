# 83. Remove Duplicates from Sorted List

## Problem Statement
Given the `head` of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list **sorted** as well.

## Examples

### Example 1:
```
Input: head = [1,1,2]
Output: [1,2]
```

### Example 2:
```
Input: head = [1,1,2,3,3]
Output: [1,2,3]
```

## Approach
We can solve this problem using a single pointer to traverse the list:
1. Start from the head of the list.
2. For each node, check if its value is equal to the value of the next node.
3. If they are equal, skip the next node by setting `current.next = current.next.next`.
4. If they are not equal, move the pointer to the next node.
5. Continue this process until we reach the end of the list.

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
    public ListNode deleteDuplicates(ListNode head) {
        ListNode current = head;
        
        while (current != null && current.next != null) {
            if (current.val == current.next.val) {
                // Skip the next node
                current.next = current.next.next;
            } else {
                // Move to the next node
                current = current.next;
            }
        }
        
        return head;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n) - We traverse the list once.
- **Space Complexity**: O(1) - We use a constant amount of extra space.

## Key Insights
1. **In-place Modification**: We modify the list in place without using extra space.
2. **Single Pass**: The solution requires only a single pass through the list.
3. **Sorted List**: Since the list is sorted, all duplicates are adjacent, making it easier to remove them in a single pass.
4. **Edge Case Handling**: The solution handles edge cases such as an empty list or a list with a single node.

## Edge Cases
1. Empty list (should return null)
2. List with a single node (should return the same list)
3. List with all nodes having the same value
4. List with no duplicates
5. List with multiple duplicates in different positions

## Follow-up
How would you modify the solution to handle an unsorted linked list and remove all duplicates?