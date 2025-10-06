# Find Median from Data Stream

**Difficulty:** Hard

## Problem Statement
The MedianFinder class is designed to efficiently add numbers from a data stream and find the median of all elements so far.

Implement the MedianFinder class:
- `MedianFinder()` initializes the MedianFinder object.
- `void addNum(int num)` adds the integer num from the data stream to the data structure.
- `double findMedian()` returns the median of all elements so far. Answers within 10^-5 of the actual answer will be accepted.

**Example:**
```
Input: ["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
[[],[1],[2],[],[3],[]]
Output: [null,null,null,1.5,null,2.0]
```

**Constraints:**
- -10^5 <= num <= 10^5
- There will be at least one element before calling findMedian.
- At most 5 * 10^4 calls will be made to addNum and findMedian.

---

## Java Solution
```java
public class MedianFinder {

	private PriorityQueue<Integer> lo = new PriorityQueue<>((a, b) -> b - a);  // max heap
	private PriorityQueue<Integer> hi = new PriorityQueue<>();  // min heap

	// Adds a number into the data structure.
	public void addNum(int num) {
		lo.offer(num);  // Add to max heap

		hi.offer(lo.poll());  // balancing step

		if (lo.size() < hi.size()) {  // maintain size property
			lo.offer(hi.poll());
		}
	}

	// Returns the median of current data stream
	public double findMedian() {
		return lo.size() > hi.size() ? lo.peek() : (lo.peek() + hi.peek()) * 0.5;
	}
}
```

---

## Approach
- Use two heaps: a max-heap for the lower half and a min-heap for the upper half of the numbers.
- Always keep the size of the max-heap equal to or one more than the min-heap.
- When adding a number, add to max-heap, then move the largest to min-heap, and rebalance if needed.
- The median is the top of the max-heap if odd, or the average of the tops if even.

---

## Time and Space Complexity
- **Time Complexity:** O(log n) per addNum, O(1) per findMedian.
- **Space Complexity:** O(n) for storing all numbers in the heaps.
