# Kth Largest Element in an Array

**Difficulty:** Medium

## Problem Statement
Given an integer array `nums` and an integer `k`, return the kth largest element in the array.

Note that it is the kth largest element in the sorted order, not the kth distinct element.

**Example:**
```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5

Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4
```

**Constraints:**
- 1 <= k <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4

---

## Java Solution
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();

        for (int num : nums) {
            minHeap.offer(num);
            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }
        return minHeap.peek();
    }
}
```

---

## Approach
- Use a min-heap (PriorityQueue) of size k to keep track of the k largest elements seen so far.
- For each number, add it to the heap. If the heap size exceeds k, remove the smallest element.
- The root of the heap (minHeap.peek()) is always the kth largest element.

---

## Time and Space Complexity
- **Time Complexity:** O(n log k), where n is the number of elements in nums (each insertion/removal is O(log k)).
- **Space Complexity:** O(k) for the heap.
