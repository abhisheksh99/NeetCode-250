# 19. Remove Nth Node From End of List
**Medium**

## Problem Statement
Given the head of a linked list, remove the n-th node from the end of the list and return its head.

## Examples
### Example 1:
- Input: head = [1,2,3,4,5], n = 2
- Output: [1,2,3,5]

### Example 2:
- Input: head = [1], n = 1
- Output: []

### Example 3:
- Input: head = [1,2], n = 1
- Output: [1]

## Constraints
- The number of nodes in the list is sz.
- 1 <= sz <= 30
- 0 <= Node.val <= 100
- 1 <= n <= sz

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
	public ListNode removeNthFromEnd(ListNode head, int n) {
		ListNode dummy=new ListNode(1);
		dummy.next=head;
		ListNode front=dummy;
		ListNode back=dummy;

		for(int i=0;i<=n;i++){
			front=front.next;
		}
		while(front!=null){
			front=front.next;
			back=back.next;
		}
		back.next=back.next.next;
		return dummy.next;
	}
}
```

Approach

- Use a dummy node to handle edge cases (removing the head).
- Use two pointers (`front` and `back`). Move `front` n+1 steps ahead.
- Move both pointers until `front` reaches the end. `back` will be just before the node to remove.
- Adjust pointers to remove the nth node from the end.
- Return the new head.

Time Complexity

O(L), where L is the length of the list.

Space Complexity

O(1), as only a few pointers are used.
