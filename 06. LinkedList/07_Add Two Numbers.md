# 2. Add Two Numbers
**Medium**

## Problem Statement
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

## Examples
### Example 1:
- Input: l1 = [2,4,3], l2 = [5,6,4]
- Output: [7,0,8]
- Explanation: 342 + 465 = 807.

### Example 2:
- Input: l1 = [0], l2 = [0]
- Output: [0]

### Example 3:
- Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
- Output: [8,9,9,9,0,0,0,1]

## Constraints
- The number of nodes in each linked list is in the range [1, 100].
- 0 <= Node.val <= 9
- It is guaranteed that the list represents a number that does not have leading zeros.

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
	public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
		ListNode dummy= new ListNode(0);
		ListNode ans= dummy;
		int carry=0;

		while(l1!=null || l2!=null||carry!=0){
			int x= (l1!=null) ? l1.val:0;
			int y= (l2!=null) ? l2.val:0;
			int sum = x+y+carry;
			carry = sum/10;
			ans.next=new ListNode(sum%10);
			ans=ans.next;

			if(l1!=null){
				l1=l1.next;
			}
			if(l2!=null){
				l2=l2.next;
			}
		}
	return dummy.next;
	}
}
```

Approach

- Use a dummy node to simplify edge cases.
- Traverse both lists, adding corresponding digits and the carry.
- Create new nodes for each digit of the result.
- Continue until both lists and carry are processed.
- Return the result starting from `dummy.next`.

Time Complexity

O(max(m, n)), where m and n are the lengths of the two lists.

Space Complexity

O(max(m, n)), for the output list.
