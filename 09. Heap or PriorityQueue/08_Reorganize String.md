# Reorganize String

**Difficulty:** Medium

## Problem Statement
Given a string s, rearrange the characters of s so that no two adjacent characters are the same. If it is not possible, return an empty string.

**Example:**
```
Input: s = "aab"
Output: "aba"

Input: s = "aaab"
Output: ""
```

**Constraints:**
- 1 <= s.length <= 500
- s consists of lowercase English letters.

---

## Java Solution
```java
public class Solution {
	public String reorganizeString(String s) {
		HashMap<Character, Integer> freqMap = new HashMap<>();
		for (char c : s.toCharArray()) {
			freqMap.put(c, freqMap.getOrDefault(c, 0) + 1);
		}

		PriorityQueue<Character> maxHeap = new PriorityQueue<>((a, b) -> freqMap.get(b) - freqMap.get(a));
		maxHeap.addAll(freqMap.keySet());

		StringBuilder res = new StringBuilder();
		while (maxHeap.size() >= 2) {
			char char1 = maxHeap.poll();
			char char2 = maxHeap.poll();

			res.append(char1);
			res.append(char2);

			freqMap.put(char1, freqMap.get(char1) - 1);
			freqMap.put(char2, freqMap.get(char2) - 1);

			if (freqMap.get(char1) > 0) maxHeap.add(char1);
			if (freqMap.get(char2) > 0) maxHeap.add(char2);
		}

		if (!maxHeap.isEmpty()) {
			char ch = maxHeap.poll();
			if (freqMap.get(ch) > 1) return "";
			res.append(ch);
		}

		return res.toString();
	}
}
```

---

## Approach
- Count the frequency of each character in the string.
- Use a max-heap to always pick the two most frequent characters and append them to the result (ensuring no two adjacent characters are the same).
- Decrease their counts and re-add them to the heap if they still have remaining occurrences.
- If one character remains, check if it can be placed without violating the rule.
- If not possible, return an empty string.

---

## Time and Space Complexity
- **Time Complexity:** O(n log k), where n is the length of the string and k is the number of unique characters (at most 26).
- **Space Complexity:** O(n) for the result and O(k) for the heap and frequency map.
