# 560. Subarray Sum Equals K
**Medium**

## Problem Statement
Given an array of integers `nums` and an integer `k`, return the total number of continuous subarrays whose sum equals to `k`.

## Examples
### Example 1:
- Input: nums = [1,1,1], k = 2
- Output: 2

### Example 2:
- Input: nums = [1,2,3], k = 3
- Output: 2

## Constraints
- 1 <= nums.length <= 2 * 10^4
- -1000 <= nums[i] <= 1000
- -10^7 <= k <= 10^7

## Solution
```java
class Solution {
	public int subarraySum(int[] nums, int k) {
		int sum = 0;
		HashMap<Integer, Integer> arr = new HashMap<>();
		arr.put(0, 1); 
		int result = 0;
        
		for (int i = 0; i < nums.length; i++) {
			sum += nums[i];
			if (arr.containsKey(sum - k)) {
				result += arr.get(sum - k);
			}
			arr.put(sum, arr.getOrDefault(sum, 0) + 1);
		}
        
		return result;
	}
}
```

Approach

- Use a HashMap to store the cumulative sum up to each index and its frequency.
- Initialize the map with (0, 1) to handle subarrays starting at index 0.
- Iterate through the array, maintaining a running sum.
- For each element, check if (current sum - k) exists in the map:
  - If yes, add its frequency to the result (number of subarrays ending at current index with sum k).
- Update the map with the current sum.
- Notes: This approach efficiently counts all subarrays with sum k in a single pass.

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(n), for the HashMap storing prefix sums.
