# 1095. Find in Mountain Array
**Hard**

## Problem Statement
(This problem is an interactive problem.)

Given a mountain array `mountainArr`, return the minimum index such that `mountainArr.get(index) == target`. If such an index does not exist, return -1.

You cannot access the mountain array directly. You may only access the array using `MountainArray.get(index)` and `MountainArray.length()`.

A mountain array is defined as an array where:
- `mountainArr.length >= 3`
- There exists some `i` with `0 < i < mountainArr.length - 1` such that:
  - `mountainArr.get(0) < mountainArr.get(1) < ... < mountainArr.get(i)`
  - `mountainArr.get(i) > mountainArr.get(i + 1) > ... > mountainArr.get(mountainArr.length - 1)`

## Examples
### Example 1:
- Input: array = [1,2,3,4,5,3,1], target = 3
- Output: 2

### Example 2:
- Input: array = [0,1,2,4,2,1], target = 3
- Output: -1

## Constraints
- 3 <= mountainArr.length() <= 10^4
- 0 <= target <= 10^9
- 0 <= mountainArr.get(index) <= 10^9

## Solution
```java
class Solution {
	public int findInMountainArray(int target, MountainArray mountainArr) {
		int n = mountainArr.length();
		int peak = findPeak(mountainArr, n);
        
		// Search ascending part
		int index = binarySearch(mountainArr, target, 0, peak, true);
		if (index != -1) return index;
        
		// Search descending part
		return binarySearch(mountainArr, target, peak + 1, n - 1, false);
	}
    
	private int findPeak(MountainArray mountainArr, int n) {
		int left = 0, right = n - 1;
		while (left < right) {
			int mid = left + (right - left) / 2;
			if (mountainArr.get(mid) > mountainArr.get(mid + 1)) {
				right = mid;
			} else {
				left = mid + 1;
			}
		}
		return left;
	}
    
	private int binarySearch(MountainArray mountainArr, int target, int left, int right, boolean asc) {
		while (left <= right) {
			int mid = left + (right - left) / 2;
			int val = mountainArr.get(mid);
			if (val == target) return mid;
			if (asc) {
				if (val < target) left = mid + 1;
				else right = mid - 1;
			} else {  // descending order
				if (val > target) left = mid + 1;
				else right = mid - 1;
			}
		}
		return -1;
	}
}
```

Approach

- Use binary search to find the peak index in the mountain array.
- Perform binary search on the ascending part (left of peak).
- If not found, perform binary search on the descending part (right of peak).
- Return the minimum index where the target is found, or -1 if not found.

Time Complexity

O(log n), where n is the length of the mountain array (each binary search is O(log n)).

Space Complexity

O(1), as only a few variables are used.
