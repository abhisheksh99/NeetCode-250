# 239. Sliding Window Maximum
**Hard**

## Problem Statement
You are given an array of integers `nums`, and there is a sliding window of size `k` which moves from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

## Examples
### Example 1:
- Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
- Output: [3,3,5,5,6,7]

### Example 2:
- Input: nums = [1], k = 1
- Output: [1]

## Constraints
- 1 <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4
- 1 <= k <= nums.length

## Solution
```java
public class Solution {
	public int[] maxSlidingWindow(int[] nums, int k) {
		if (nums == null || nums.length == 0 || k <= 0) {
			return new int[0];
		}

		int n = nums.length;
		int[] result = new int[n - k + 1];
		Deque<Integer> deque = new LinkedList<>();

		for (int i = 0; i < n; i++) {
			// Remove indices that are out of the current window
			while (!deque.isEmpty() && deque.peek() < i - k + 1) {
				deque.poll();
			}

			// Remove indices whose corresponding values are less than nums[i]
			while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
				deque.pollLast();
			}

			// Add the current index to the deque
			deque.offer(i);

			// Add the maximum element of the current window to the result
			if (i >= k - 1) {
				result[i - k + 1] = nums[deque.peek()];
			}
		}

		return result;
	}
}
```

Approach

- Use a double-ended queue (deque) to store indices of useful elements for each window.
- Remove indices from the front if they are out of the current window.
- Remove indices from the back if their corresponding values are less than the current value.
- The front of the deque always contains the index of the maximum element for the current window.
- After the first `k-1` elements, add the maximum for each window to the result array.

Time Complexity

O(n), where n is the length of `nums` (each element is added and removed from the deque at most once).

Space Complexity

O(k), for the deque storing indices within the window.
