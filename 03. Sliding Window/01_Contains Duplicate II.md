# 219. Contains Duplicate II
**Easy**

## Problem Statement
Given an integer array `nums` and an integer `k`, return true if there are two distinct indices `i` and `j` in the array such that `nums[i] == nums[j]` and `abs(i - j) <= k`.

## Examples
### Example 1:
- Input: nums = [1,2,3,1], k = 3
- Output: true

### Example 2:
- Input: nums = [1,0,1,1], k = 1
- Output: true

### Example 3:
- Input: nums = [1,2,3,1,2,3], k = 2
- Output: false

## Constraints
- 1 <= nums.length <= 10^5
- -10^9 <= nums[i] <= 10^9
- 0 <= k <= 10^5

## Solution
```java
public class Solution {
	public boolean containsNearbyDuplicate(int[] nums, int k) {
		Map<Integer, Integer> map = new HashMap<>();
        
		for (int i = 0; i < nums.length; i++) {
			if (map.containsKey(nums[i]) && i - map.get(nums[i]) <= k) {
				return true;
			}
			map.put(nums[i], i);
		}
        
		return false;
	}
}
```

Approach

- Use a HashMap to store the most recent index of each number.
- Iterate through the array:
  - For each number, check if it exists in the map and if the distance between indices is at most k.
  - If so, return true.
  - Otherwise, update the map with the current index.
- If no such pair is found, return false.
- Notes: This approach efficiently checks for nearby duplicates using a sliding window of size k.

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(min(n, k)), as the map stores at most k elements at any time.
