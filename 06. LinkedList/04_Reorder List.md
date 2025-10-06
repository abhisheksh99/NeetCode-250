# 143. Reorder List
**Medium**

## Problem Statement
You are given the head of a singly linked list. Reorder the list so that the nodes are arranged in the following order:
L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...

You must do this in-place without altering the nodes' values.

## Examples
### Example 1:
- Input: head = [1,2,3,4]
- Output: [1,4,2,3]

### Example 2:
- Input: head = [1,2,3,4,5]
- Output: [1,5,2,4,3]

## Constraints
- The number of nodes in the list is in the range [1, 5 * 10^4].
- 1 <= Node.val <= 1000

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
	public void reorderList(ListNode head) {
		if (head == null) {
			return;
		}

		ListNode slow = head, fast = head;

		// Find the middle of the list
		while (fast != null && fast.next != null) {
			slow = slow.next;
			fast = fast.next.next;
		}

		// Reverse the second half of the list
		ListNode prev = null, curr = slow, temp;
		while (curr != null) {
			temp = curr.next;
			curr.next = prev;
			prev = curr;
			curr = temp;
		}

		// Merge the two halves
		ListNode first = head, second = prev;
		while (second.next != null) {
			temp = first.next;
			first.next = second;
			first = temp;

			temp = second.next;
			second.next = first;
			second = temp;
		}
	}
}
```

Approach

- Use the slow and fast pointer technique to find the middle of the list.
- Reverse the second half of the list in-place.
- Merge the two halves by alternating nodes from each half.
- All operations are done in-place without using extra space for node values.

Time Complexity

O(n), where n is the number of nodes in the list.

Space Complexity

O(1), as all operations are done in-place.
