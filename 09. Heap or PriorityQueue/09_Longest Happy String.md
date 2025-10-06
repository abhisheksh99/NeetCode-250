# Longest Happy String

**Difficulty:** Medium

## Problem Statement
Given three integers a, b, and c, return the longest possible string that can be constructed using 'a', 'b', and 'c' such that no three consecutive characters are the same, and using at most a 'a's, b 'b's, and c 'c's.

**Example:**
```
Input: a = 1, b = 1, c = 7
Output: "ccaccbcc"
```

**Constraints:**
- 0 <= a, b, c <= 100

---

## Java Solution
```java
public String longestDiverseString(int a, int b, int c) {
	PriorityQueue<int[]> pq = new PriorityQueue<>((x, y) -> y[1] - x[1]);
	if (a > 0) pq.offer(new int[]{'a', a});
	if (b > 0) pq.offer(new int[]{'b', b});
	if (c > 0) pq.offer(new int[]{'c', c});

	StringBuilder sb = new StringBuilder();

	while (!pq.isEmpty()) {
		int[] first = pq.poll();

		int len = sb.length();
		// Check if last two chars are same as current char
		if (len >= 2 && sb.charAt(len - 1) == first[0] && sb.charAt(len - 2) == first[0]) {
			if (pq.isEmpty()) break; // No alternative available
			int[] second = pq.poll();
			sb.append((char) second[0]);
			second[1]--;
			if (second[1] > 0) pq.offer(second);
			pq.offer(first); // Put the skipped char back
		} else {
			sb.append((char) first[0]);
			first[1]--;
			if (first[1] > 0) pq.offer(first);
		}
	}

	return sb.toString();
}
```

---

## Approach
- Use a max-heap to always pick the character with the highest remaining count.
- If the last two characters in the result are the same as the current character, pick the next most frequent character (if available).
- Decrease the count and re-add characters to the heap as needed.
- Continue until no more valid characters can be added.

---

## Time and Space Complexity
- **Time Complexity:** O(a + b + c) (each character is processed at most once per count).
- **Space Complexity:** O(a + b + c) for the result and heap.
