# 88. Merge Sorted Array
**Easy**

## Problem Statement
You are given two integer arrays `nums1` and `nums2`, sorted in non-decreasing order, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.
Merge `nums2` into `nums1` as one sorted array. The final sorted array should not be returned by the function, but instead be stored inside the array `nums1`. To accommodate this, `nums1` has a length of `m + n`, with the first `m` elements initialized and the last `n` elements set to 0 and should be ignored. `nums2` has a length of `n`.

## Examples
### Example 1:
- Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
- Output: [1,2,2,3,5,6]

### Example 2:
- Input: nums1 = [1], m = 1, nums2 = [], n = 0
- Output: [1]

### Example 3:
- Input: nums1 = [0], m = 0, nums2 = [1], n = 1
- Output: [1]

## Constraints
- nums1.length == m + n
- nums2.length == n
- 0 <= m, n <= 200
- 1 <= m + n <= 200
- -10^9 <= nums1[i], nums2[j] <= 10^9

## Solution
```java
class Solution {
	public void merge(int[] nums1, int m, int[] nums2, int n) {
		int i = m - 1, j = n - 1, k = m + n - 1;
        
		while (j >= 0) {
			if (i >= 0 && nums1[i] > nums2[j]) {
				nums1[k] = nums1[i];
				k--;
				i--;
			} else {
				nums1[k] = nums2[j];
				k--;
				j--;
			}
		}
	}
}
```

Approach

- Use three pointers: `i` for the end of initialized elements in `nums1`, `j` for the end of `nums2`, and `k` for the end of `nums1`'s total length.
- Compare elements from the end of both arrays, placing the larger one at the end of `nums1`.
- Move the pointers accordingly until all elements from `nums2` are merged.
- Notes: This approach avoids overwriting elements in `nums1` that haven't been checked yet.

Time Complexity

O(m + n), where m and n are the lengths of the two arrays.

Space Complexity

O(1), as merging is done in-place.
