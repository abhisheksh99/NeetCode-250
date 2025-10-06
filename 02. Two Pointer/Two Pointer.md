
# Two Pointer Technique

The two-pointer technique is a powerful approach for solving array and string problems efficiently, especially when the input is sorted or when you need to scan from both ends.

## Generic Java Template

```java
// Example: Find if there exists a pair with sum == target in a sorted array
public boolean twoPointerTemplate(int[] nums, int target) {
	int left = 0;
	int right = nums.length - 1;
	while (left < right) {
		int sum = nums[left] + nums[right];
		if (sum == target) {
			return true; // or return indices, etc.
		} else if (sum < target) {
			left++;
		} else {
			right--;
		}
	}
	return false;
}
```

### Variations
- Moving both pointers from the start (for partitioning, removing duplicates, etc.)
- One pointer scans, the other lags behind (for sliding window problems)
- Both pointers move towards each other (for palindrome checks, etc.)

## Time and Space Complexity

- **Time Complexity:** O(n), where n is the length of the array or string (each pointer moves at most n times).
- **Space Complexity:** O(1), as only a constant number of variables are used.


## Interview Tips
- Use when the problem involves searching for pairs, subarrays, or substrings with certain properties.
- Works best on sorted arrays or when the problem allows you to sort the input first.
- Common in problems like: Two Sum II, 3Sum, Valid Palindrome, Container With Most Water, etc.

---

## More Two Pointer Variations

### 1. Fast and Slow Pointers (Tortoise and Hare)
Used for cycle detection, finding the middle of a linked list, or removing duplicates in-place.

```java
// Example: Detect cycle in a linked list
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
	slow = slow.next;
	fast = fast.next.next;
	if (slow == fast) return true;
}
return false;
```

### 2. Sliding Window (Dynamic Window)
One pointer expands the window, the other contracts it to maintain a condition (sum, unique chars, etc.).

```java
// Example: Longest substring with at most K distinct characters
int left = 0;
for (int right = 0; right < s.length(); right++) {
	// expand window by moving right
	// shrink window by moving left as needed
}
```

### 3. Partitioning/Segregation
Both pointers start at the beginning; one scans, the other marks the boundary for a condition (e.g., move zeros to the end).

```java
// Example: Move all zeros to the end
int insertPos = 0;
for (int num : nums) {
	if (num != 0) nums[insertPos++] = num;
}
```

### 4. Bidirectional Search
One pointer from the start, one from the end, used for palindrome checks, or reversing arrays in-place.

```java
// Example: Reverse an array in-place
int left = 0, right = arr.length - 1;
while (left < right) {
	int temp = arr[left];
	arr[left++] = arr[right];
	arr[right--] = temp;
}
```
