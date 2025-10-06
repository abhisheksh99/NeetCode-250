## Generic Sliding Window Template (Java)

```java
// Generic template for sliding window problems
public int slidingWindowTemplate(int[] nums) {
	int left = 0, right = 0;
	int result = 0;
	// Use a HashMap or frequency array if you need to track counts or unique elements
	Map<Integer, Integer> window = new HashMap<>();

	while (right < nums.length) {
		// Expand the window by including nums[right]
		int curr = nums[right];
		window.put(curr, window.getOrDefault(curr, 0) + 1);

		// While window is invalid (custom condition), shrink from the left
		while (/* window is invalid */) {
			int leftVal = nums[left];
			window.put(leftVal, window.get(leftVal) - 1);
			if (window.get(leftVal) == 0) window.remove(leftVal);
			left++;
		}

		// Update result if needed (e.g., max/min window size, count, etc.)
		// result = ...

		right++;
	}
	return result;
}
```

**How to use:**
- Customize the window expansion and shrinking conditions for your specific problem.
- Use a HashMap, Set, or frequency array to track window properties as needed.
- Update the result inside the main loop as required (max/min/length/count/etc.).

# Sliding Window Technique

The sliding window technique is a powerful approach for solving problems involving subarrays or substrings, especially when you need to find the optimal or required property in a contiguous segment.

## When to Use
- When the problem asks for the maximum/minimum/average/sum/length of a subarray or substring.
- When you need to process all contiguous subarrays of a given size (fixed window) or with a property (dynamic window).

## Fixed-Size Sliding Window (Java Template)

```java
// Example: Maximum sum of subarray of size k
public int maxSumFixedWindow(int[] nums, int k) {
	int maxSum = 0, windowSum = 0;
	for (int i = 0; i < nums.length; i++) {
		windowSum += nums[i];
		if (i >= k - 1) {
			maxSum = Math.max(maxSum, windowSum);
			windowSum -= nums[i - k + 1];
		}
	}
	return maxSum;
}
```

## Dynamic-Size Sliding Window (Java Template)

```java
// Example: Longest substring with at most K distinct characters
public int lengthOfLongestSubstringKDistinct(String s, int k) {
	Map<Character, Integer> map = new HashMap<>();
	int left = 0, maxLen = 0;
	for (int right = 0; right < s.length(); right++) {
		char c = s.charAt(right);
		map.put(c, map.getOrDefault(c, 0) + 1);
		while (map.size() > k) {
			char leftChar = s.charAt(left);
			map.put(leftChar, map.get(leftChar) - 1);
			if (map.get(leftChar) == 0) map.remove(leftChar);
			left++;
		}
		maxLen = Math.max(maxLen, right - left + 1);
	}
	return maxLen;
}
```

## Time and Space Complexity

- **Time Complexity:** O(n), where n is the length of the array or string (each element is processed at most twice).
- **Space Complexity:**
  - Fixed window: O(1) extra space (just a few variables).
  - Dynamic window: O(k) or O(m), where k is the number of unique elements in the window, or m is the alphabet size (for character problems).

## Interview Tips
- Always clarify if the window size is fixed or dynamic.
- For dynamic windows, use a HashMap or frequency array to track window properties.
- Practice problems: Longest Substring Without Repeating Characters, Minimum Window Substring, Maximum Sum Subarray of Size K, etc.
