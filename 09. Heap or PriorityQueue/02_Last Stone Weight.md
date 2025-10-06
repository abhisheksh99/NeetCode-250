# Last Stone Weight

**Difficulty:** Easy

## Problem Statement
You are given an array of integers `stones` where `stones[i]` is the weight of the i-th stone.

We repeatedly smash the two heaviest stones together. If they are equal, both are destroyed. If they are not equal, the smaller one is destroyed and the larger one is replaced by the difference. This process is repeated until there is at most one stone left.

Return the weight of the last remaining stone. If there are no stones left, return 0.

**Example:**
```
Input: stones = [2,7,4,1,8,1]
Output: 1
Explanation:
Smash 8 and 7 => 1, stones = [2,4,1,1,1]
Smash 4 and 2 => 2, stones = [2,1,1,1]
Smash 2 and 1 => 1, stones = [1,1,1]
Smash 1 and 1 => 0, stones = [1]
Last stone = 1
```

**Constraints:**
- 1 <= stones.length <= 30
- 1 <= stones[i] <= 1000

---

## Java Solution
```java
class Solution {
	public int lastStoneWeight(int[] stones) {
		PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);

		for (int stone : stones) {
			maxHeap.add(stone);
		}

		while (maxHeap.size() > 1) {
			int y = maxHeap.poll();
			int x = maxHeap.poll();
			if (x != y) {
				maxHeap.add(y - x);
			}
		}
		return maxHeap.isEmpty() ? 0 : maxHeap.poll();
	}
}
```

---

## Approach
- Use a max-heap (PriorityQueue with reverse order comparator) to always access the two largest stones.
- Repeatedly remove the two largest stones, smash them, and if the result is nonzero, add the difference back to the heap.
- Continue until one or zero stones remain.
- Return the last stone's weight or 0 if none remain.

---

## Time and Space Complexity
- **Time Complexity:** O(n log n), where n is the number of stones (each insertion/removal is O(log n)).
- **Space Complexity:** O(n) for the heap.
