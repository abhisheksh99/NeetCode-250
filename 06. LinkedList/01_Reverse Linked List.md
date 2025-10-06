# 206. Reverse Linked List
**Easy**

## Problem Statement
Given the head of a singly linked list, reverse the list, and return the reversed list.

## Examples
### Example 1:
- Input: head = [1,2,3,4,5]
- Output: [5,4,3,2,1]

### Example 2:
- Input: head = [1,2]
- Output: [2,1]

### Example 3:
- Input: head = []
- Output: []

## Constraints
- The number of nodes in the list is the range [0, 5000].
- -5000 <= Node.val <= 5000

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
	public ListNode reverseList(ListNode head) {
		ListNode prev=null;
		ListNode curr=head;

		while(curr!=null){
			ListNode temp=curr.next;
			curr.next=prev;
			prev=curr;
			curr=temp;
		}
		return prev;
	}
}
```

Approach

- Use two pointers, `prev` and `curr`, to reverse the list in-place.
- Iterate through the list, reversing the `next` pointer of each node.
- At the end, `prev` will point to the new head of the reversed list.

Time Complexity

O(n), where n is the number of nodes in the list.

Space Complexity

O(1), as the reversal is done in-place.
