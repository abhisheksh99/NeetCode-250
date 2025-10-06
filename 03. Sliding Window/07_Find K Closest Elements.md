# 658. Find K Closest Elements
**Medium**

## Problem Statement
Given a sorted integer array `arr`, two integers `k` and `x`, return the `k` closest integers to `x` in the array. The result should also be sorted in ascending order.

An integer `a` is closer to `x` than an integer `b` if:
- |a - x| < |b - x|, or
- |a - x| == |b - x| and a < b

## Examples
### Example 1:
- Input: arr = [1,2,3,4,5], k = 4, x = 3
- Output: [1,2,3,4]

### Example 2:
- Input: arr = [1,2,3,4,5], k = 4, x = -1
- Output: [1,2,3,4]

## Constraints
- 1 <= k <= arr.length
- 1 <= arr.length <= 10^4
- arr is sorted in ascending order
- -10^4 <= arr[i], x <= 10^4

## Solution
```java
class Solution {
	public List<Integer> findClosestElements(int[] arr, int k, int x) {
		int left = 0;
		int right = arr.length - 1;

		// Shrink window until size is k
		while (right - left + 1 > k) {
			if (Math.abs(arr[left] - x) > Math.abs(arr[right] - x)) {
				left++;
			} else {
				right--;
			}
		}

		// Collect results
		List<Integer> result = new ArrayList<>();
		for (int i = left; i <= right; i++) {
			result.add(arr[i]);
		}

		return result;
	}
}
```

Approach

- Use two pointers (`left` and `right`) to maintain a window over the array.
- Shrink the window from the side with the element farther from `x` until the window size is `k`.
- The remaining window contains the `k` closest elements.
- Collect and return the elements in the window as the result.

Time Complexity

O(n), where n is the length of `arr` (since each element is considered at most once).

Space Complexity

O(k), for storing the result list of size `k`.
