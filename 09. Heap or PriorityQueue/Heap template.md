---

## Array-based Min-Heap Implementation (Java)
```java
class MinHeap {
	private int[] heap;
	private int size;
	private int capacity;

	public MinHeap(int capacity) {
		this.capacity = capacity;
		heap = new int[capacity + 1]; // 1-based index
		size = 0;
	}

	public void insert(int val) {
		if (size >= capacity) throw new IllegalStateException("Heap is full");
		heap[++size] = val;
		swim(size);
	}

	public int poll() {
		if (isEmpty()) throw new NoSuchElementException("Heap is empty");
		int min = heap[1];
		heap[1] = heap[size--];
		sink(1);
		return min;
	}

	public int peek() {
		if (isEmpty()) throw new NoSuchElementException("Heap is empty");
		return heap[1];
	}

	public boolean isEmpty() { return size == 0; }

	private void swim(int k) {
		while (k > 1 && heap[k/2] > heap[k]) {
			swap(k, k/2);
			k /= 2;
		}
	}

	private void sink(int k) {
		while (2*k <= size) {
			int j = 2*k;
			if (j < size && heap[j] > heap[j+1]) j++;
			if (heap[k] <= heap[j]) break;
			swap(k, j);
			k = j;
		}
	}

	private void swap(int i, int j) {
		int tmp = heap[i]; heap[i] = heap[j]; heap[j] = tmp;
	}
}
```

## Array-based Max-Heap Implementation (Java)
```java
class MaxHeap {
	private int[] heap;
	private int size;
	private int capacity;

	public MaxHeap(int capacity) {
		this.capacity = capacity;
		heap = new int[capacity + 1]; // 1-based index
		size = 0;
	}

	public void insert(int val) {
		if (size >= capacity) throw new IllegalStateException("Heap is full");
		heap[++size] = val;
		swim(size);
	}

	public int poll() {
		if (isEmpty()) throw new NoSuchElementException("Heap is empty");
		int max = heap[1];
		heap[1] = heap[size--];
		sink(1);
		return max;
	}

	public int peek() {
		if (isEmpty()) throw new NoSuchElementException("Heap is empty");
		return heap[1];
	}

	public boolean isEmpty() { return size == 0; }

	private void swim(int k) {
		while (k > 1 && heap[k/2] < heap[k]) {
			swap(k, k/2);
			k /= 2;
		}
	}

	private void sink(int k) {
		while (2*k <= size) {
			int j = 2*k;
			if (j < size && heap[j] < heap[j+1]) j++;
			if (heap[k] >= heap[j]) break;
			swap(k, j);
			k = j;
		}
	}

	private void swap(int i, int j) {
		int tmp = heap[i]; heap[i] = heap[j]; heap[j] = tmp;
	}
}
```

# Heap (Priority Queue) - Complete Guide & Templates

## What is a Heap?
A heap is a specialized tree-based data structure that satisfies the heap property:
- **Min-Heap:** The parent node is always less than or equal to its children (smallest element at the root).
- **Max-Heap:** The parent node is always greater than or equal to its children (largest element at the root).

Heaps are commonly used to implement priority queues.

## Use Cases
- Finding the k largest/smallest elements
- Scheduling and simulation
- Dijkstra's and Prim's algorithms
- Merging k sorted lists
- Median maintenance

---

## Java Heap Templates

### Min-Heap (Default in Java)
```java
// Min-Heap using PriorityQueue
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.offer(5);
minHeap.offer(2);
minHeap.offer(8);
int smallest = minHeap.poll(); // 2
```

### Max-Heap
```java
// Max-Heap using PriorityQueue with custom comparator
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
maxHeap.offer(5);
maxHeap.offer(2);
maxHeap.offer(8);
int largest = maxHeap.poll(); // 8
```

### Custom Objects in Heap
```java
// For custom objects, provide a Comparator
PriorityQueue<Pair> minHeap = new PriorityQueue<>((a, b) -> a.value - b.value);
// or for max-heap: (a, b) -> b.value - a.value
```

---

## Heap Operations
- **Insert:** O(log n)
- **Remove (poll):** O(log n)
- **Peek (top):** O(1)
- **Heapify (build heap):** O(n)
- **Update:** O(log n) (remove and re-insert)

---

## Space and Time Complexity
- **Space:** O(n), where n is the number of elements in the heap.
- **Insert/Remove:** O(log n)
- **Peek:** O(1)
- **Heapify:** O(n)

---

## Heap Interview Tips
- Know how to use both min-heap and max-heap in Java.
- For k largest/smallest problems, use a heap of size k.
- For custom objects, always implement Comparator or Comparable.
- For median maintenance, use two heaps (max-heap for lower half, min-heap for upper half).
- Understand how to remove arbitrary elements (not O(1) in Java's PriorityQueue).
- Be able to implement a heap from scratch (array-based binary heap) if asked.

---

## Array-based Heap Implementation (for reference)
```java
// 1-based index for easier parent/child calculation
class BinaryHeap {
	int[] heap;
	int size;
	public BinaryHeap(int capacity) {
		heap = new int[capacity + 1];
		size = 0;
	}
	void insert(int val) {
		heap[++size] = val;
		swim(size);
	}
	int poll() {
		int top = heap[1];
		heap[1] = heap[size--];
		sink(1);
		return top;
	}
	void swim(int k) {
		while (k > 1 && heap[k/2] > heap[k]) {
			swap(k, k/2);
			k /= 2;
		}
	}
	void sink(int k) {
		while (2*k <= size) {
			int j = 2*k;
			if (j < size && heap[j] > heap[j+1]) j++;
			if (heap[k] <= heap[j]) break;
			swap(k, j);
			k = j;
		}
	}
	void swap(int i, int j) {
		int tmp = heap[i]; heap[i] = heap[j]; heap[j] = tmp;
	}
}
```

---

## Summary
- Use PriorityQueue for most heap problems in Java.
- Know the difference between min-heap and max-heap.
- Understand time/space complexity and when to use a heap for optimal performance.
