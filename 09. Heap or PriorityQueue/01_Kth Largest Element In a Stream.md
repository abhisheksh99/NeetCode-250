# Kth Largest Element in a Stream

**Difficulty:** Medium

## Problem Statement
Design a class to find the kth largest element in a stream. Note that it is the kth largest element in the sorted order, not the kth distinct element.

Implement `KthLargest` class:
- `KthLargest(int k, int[] nums)` Initializes the object with the integer k and the stream of integers nums.
- `int add(int val)` Appends the integer val to the stream and returns the element representing the kth largest element in the stream.

**Example:**
```
Input:
["KthLargest", "add", "add", "add", "add", "add"]
[[3, [4, 5, 8, 2]], [3], [5], [10], [9], [4]]
Output:
[null, 4, 5, 5, 8, 8]
```

**Constraints:**
- 1 <= k <= 10^4
- 0 <= nums.length <= 10^4
- -10^4 <= nums[i], val <= 10^4
- At most 10^4 calls will be made to add.

---

## Java Solution
```java
class KthLargest {

	private PriorityQueue<Integer> minHeap;
	private int k;

	public KthLargest(int k, int[] nums) {
		this.k = k;
		this.minHeap = new PriorityQueue<>();
		for (int num : nums) {
			minHeap.offer(num);
			if (minHeap.size() > k) {
				minHeap.poll();
			}
		}
	}

	public int add(int val) {
		minHeap.offer(val);
		if (minHeap.size() > k) {
			minHeap.poll();
		}
		return minHeap.peek();
	}
}
```

---

## Approach
- Use a min-heap (PriorityQueue) of size k to keep track of the k largest elements seen so far.
- For each new element, add it to the heap. If the heap size exceeds k, remove the smallest element.
- The root of the heap (minHeap.peek()) is always the kth largest element.

---

## Time and Space Complexity
- **Time Complexity:** O(log k) per add operation (heap insertion and removal).
- **Space Complexity:** O(k) for storing the heap.
