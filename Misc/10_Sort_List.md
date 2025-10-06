# 148. Sort List

## Problem Statement
Given the `head` of a linked list, return the list after sorting it in ascending order.

## Examples

### Example 1:
```
Input: head = [4,2,1,3]
Output: [1,2,3,4]
```

### Example 2:
```
Input: head = [-1,5,3,4,0]
Output: [-1,0,3,4,5]
```

### Example 3:
```
Input: head = []
Output: []
```

## Approach
We can solve this problem using the merge sort algorithm, which is well-suited for linked lists due to its O(n log n) time complexity and O(1) space complexity (excluding the recursion stack). The approach involves:
1. **Splitting the list**: Find the middle of the list using the slow and fast pointer technique and split the list into two halves.
2. **Recursively sorting**: Recursively sort the two halves.
3. **Merging**: Merge the two sorted halves back together.

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
    public ListNode sortList(ListNode head) {
        // Base case: if the list is empty or has only one node
        if (head == null || head.next == null) {
            return head;
        }
        
        // Find the middle of the list
        ListNode mid = findMiddle(head);
        ListNode left = head;
        ListNode right = mid.next;
        mid.next = null; // Split the list into two halves
        
        // Recursively sort the two halves
        left = sortList(left);
        right = sortList(right);
        
        // Merge the sorted halves
        return merge(left, right);
    }
    
    // Helper method to find the middle of the list
    private ListNode findMiddle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head.next;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        return slow;
    }
    
    // Helper method to merge two sorted lists
    private ListNode merge(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode current = dummy;
        
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                current.next = l1;
                l1 = l1.next;
            } else {
                current.next = l2;
                l2 = l2.next;
            }
            current = current.next;
        }
        
        // Attach the remaining elements of l1 or l2
        if (l1 != null) {
            current.next = l1;
        } else {
            current.next = l2;
        }
        
        return dummy.next;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n log n) - The merge sort algorithm divides the list into two halves in each recursive call and merges them in O(n) time.
- **Space Complexity**: O(log n) - The space is used for the recursion stack. The merge operation is done in-place, so it uses O(1) extra space.

## Key Insights
1. **Divide and Conquer**: The problem is divided into smaller subproblems, solved independently, and combined.
2. **Efficient Splitting**: The middle of the list is found using the slow and fast pointer technique, ensuring the list is split into two nearly equal parts.
3. **In-place Merging**: The merge operation is done in-place, which helps in maintaining the O(1) space complexity (excluding the recursion stack).

## Edge Cases
1. Empty list (should return null)
2. List with a single node (should return the same list)
3. List with all nodes having the same value
4. List already sorted in ascending order
5. List sorted in descending order

## Follow-up
How would you modify the solution to sort the linked list in O(n log n) time and O(1) memory (i.e., constant space)?