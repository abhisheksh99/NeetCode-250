# 234. Palindrome Linked List

## Problem Statement
Given the `head` of a singly linked list, return `true` if it is a palindrome or `false` otherwise.

A palindrome is a sequence that reads the same forward and backward.

## Examples

### Example 1:
```
Input: head = [1,2,2,1]
Output: true
```

### Example 2:
```
Input: head = [1,2]
Output: false
```

## Approach
We can solve this problem by finding the middle of the linked list, reversing the second half, and then comparing it with the first half. If they match, the list is a palindrome.

1. **Find the Middle**: Use the slow and fast pointer technique to find the middle of the linked list.
2. **Reverse the Second Half**: Reverse the second half of the linked list starting from the middle.
3. **Compare Halves**: Compare the first half with the reversed second half. If all values match, it's a palindrome.
4. **Restore the List (Optional)**: If required, we can restore the list by reversing the second half again, though this is often not necessary.

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
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) {
            return true;
        }
        
        // Step 1: Find the middle of the linked list
        ListNode slow = head;
        ListNode fast = head;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        // Step 2: Reverse the second half
        ListNode prev = null;
        ListNode curr = slow;
        
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        
        // Step 3: Compare the first and second half
        ListNode first = head;
        ListNode second = prev; // prev is now the head of the reversed second half
        
        while (second != null) {
            if (first.val != second.val) {
                return false;
            }
            first = first.next;
            second = second.next;
        }
        
        return true;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n) - We traverse the list twice: once to find the middle and once to compare the two halves.
- **Space Complexity**: O(1) - We use a constant amount of extra space.

## Key Insights
1. **Two-pointer Technique**: We use the slow and fast pointer technique to find the middle of the linked list efficiently.
2. **In-place Reversal**: We reverse the second half of the list in place, which allows us to compare it with the first half without using extra space.
3. **No Extra Space**: The solution is optimized to use constant extra space, making it efficient in terms of memory usage.

## Edge Cases
1. Empty list (should return true)
2. List with a single node (should return true)
3. List with two nodes (should check if they are equal)
4. List with an odd number of nodes (the middle node can be ignored)
5. List with an even number of nodes (both halves should match exactly)

## Follow-up
How would you modify the solution if you were required to restore the original linked list after checking if it's a palindrome?