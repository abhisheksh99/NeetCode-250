# Insert Greatest Common Divisors in Linked List

**Difficulty:** Easy

## Problem Statement  
Given the head of a linked list where each node contains an integer value, insert a new node between every two adjacent nodes containing the greatest common divisor (GCD) of the values of those nodes.

Return the head of the modified linked list.

**Example:**  
Input: head = [18, 6, 10, 3]  
Output: [18, 6, 6, 2, 10, 1, 3]

Explanation:  
- Between 18 and 6, gcd is 6, so insert 6.  
- Between 6 and 10, gcd is 2, so insert 2.  
- Between 10 and 3, gcd is 1, so insert 1.

**Constraints:**  
- The number of nodes in the list is in the range [1, 5000].  
- 1 <= Node.val <= 1000.

---

## Approach  
- Traverse the linked list from head to tail.  
- For every pair of adjacent nodes, calculate the gcd of their values.  
- Create a new node with the gcd value and insert it between the two nodes.  
- Move the current pointer two steps forward (past the inserted node).  
- Continue until the end of the list.

---

## Time and Space Complexity  
- **Time Complexity:** O(n log m), where n is the number of nodes and m is the max node value (due to gcd calculation).  
- **Space Complexity:** O(1), as insertion is done in-place.

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
public class Solution {
    public ListNode insertGreatestCommonDivisors(ListNode head) {
        if (head == null) return null;

        ListNode cur = head;

        while (cur.next != null) {
            int n1 = cur.val, n2 = cur.next.val;
            int gcdValue = gcd(n1, n2);
            ListNode newNode = new ListNode(gcdValue, cur.next);
            cur.next = newNode;
            cur = newNode.next;
        }

        return head;
    }

    private int gcd(int a, int b) {
        while (b > 0) {
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }
}
```