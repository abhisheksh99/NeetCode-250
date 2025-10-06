# K Closest Points to Origin

**Difficulty:** Medium

## Problem Statement
Given an array of points where points[i] = [xi, yi] represents a point on the X-Y plane and an integer k, return the k closest points to the origin (0, 0).

The distance between two points on the X-Y plane is the Euclidean distance (but you only need to compare squared distances).

**Example:**
```
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation: The distance from (1,3) to the origin is sqrt(10).
The distance from (-2,2) to the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2,2) is closer.
```

**Constraints:**
- 1 <= k <= points.length <= 10^4
- -10^4 < xi, yi < 10^4

---

## Java Solution
```java
class Solution {
	public int[][] kClosest(int[][] points, int k) {
		PriorityQueue<int[]> maxHeap = new PriorityQueue<>(
			(a, b) -> Integer.compare(b[0]*b[0] + b[1]*b[1], a[0]*a[0] + a[1]*a[1])
		);

		for (int[] point : points) {
			maxHeap.offer(point);
			if (maxHeap.size() > k) {
				maxHeap.poll();
			}
		}

		int[][] res = new int[k][2];
		int i = 0;
		while (!maxHeap.isEmpty()) {
			res[i] = maxHeap.poll();
			i++;
		}
		return res;
	}
}
```

---

## Approach
- Use a max-heap (PriorityQueue) of size k to keep track of the k closest points to the origin.
- For each point, calculate its squared distance to the origin and add it to the heap.
- If the heap size exceeds k, remove the farthest point.
- At the end, the heap contains the k closest points.

---

## Time and Space Complexity
- **Time Complexity:** O(n log k), where n is the number of points (each insertion/removal is O(log k)).
- **Space Complexity:** O(k) for the heap.
