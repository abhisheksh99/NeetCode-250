# 141. Linked List Cycle
**Easy**

## Problem Statement
Given the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer.
Return true if there is a cycle in the linked list. Otherwise, return false.

## Examples
### Example 1:
- Input: head = [3,2,0,-4], pos = 1 (tail connects to node index 1)
- Output: true

### Example 2:
- Input: head = [1,2], pos = 0 (tail connects to node index 0)
- Output: true

### Example 3:
- Input: head = [1], pos = -1 (no cycle)
- Output: false

## Constraints
- The number of nodes in the list is in the range [0, 10^4].
- -10^5 <= Node.val <= 10^5
- pos is -1 or a valid index in the linked list.

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
	public boolean hasCycle(ListNode head) {
		ListNode slow=head;
		ListNode fast=head;

		while(fast!=null && fast.next!=null){
			slow=slow.next;
			fast=fast.next.next;

			if(slow==fast) return true;
		}
		return false;
	}
}
```

Approach

- Use two pointers (slow and fast) to traverse the list.
- Move slow by one step and fast by two steps.
- If they ever meet, there is a cycle.
- If fast reaches the end, there is no cycle.

Time Complexity

O(n), where n is the number of nodes in the list.

Space Complexity

O(1), as only two pointers are used.
