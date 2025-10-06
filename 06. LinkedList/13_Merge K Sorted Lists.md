# 23. Merge k Sorted Lists
**Hard**

## Problem Statement
You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

## Examples
### Example 1:
- Input: lists = [[1,4,5],[1,3,4],[2,6]]
- Output: [1,1,2,3,4,4,5,6]
- Explanation: The linked-lists are:
  [
    1->4->5,
    1->3->4,
    2->6
  ]
  merging them into one sorted list: 1->1->2->3->4->4->5->6

### Example 2:
- Input: lists = []
- Output: []

### Example 3:
- Input: lists = [[]]
- Output: []

## Constraints
- k == lists.length
- 0 <= k <= 10^4
- 0 <= lists[i].length <= 500
- -10^4 <= lists[i][j] <= 10^4
- lists[i] is sorted in ascending order.
- The sum of lists[i].length will not exceed 10^4.

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
    public ListNode mergeKLists(ListNode[] lists) {
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        for (ListNode list : lists) {
            while (list != null) {
                minHeap.add(list.val);
                list = list.next;
            }
        }
        ListNode dummy = new ListNode(0);
        ListNode current = dummy;
        while (!minHeap.isEmpty()) {
            current.next = new ListNode(minHeap.poll());
            current = current.next;
        }
        return dummy.next;
    }
}
```

Approach
- Use a min-heap (priority queue) to collect all node values from all lists.
- Extract the minimum value from the heap and build the merged sorted list.
- This approach is simple but not the most optimal in terms of time complexity.

Time Complexity
- O(N log N), where N is the total number of nodes (sum of all list lengths).

Space Complexity
- O(N), for storing all node values in the heap and the output list.
