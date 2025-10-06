# 229. Majority Element II
**Medium**

## Problem Statement
Given an integer array of size n, find all elements that appear more than ⌊ n/3 ⌋ times.

## Examples
### Example 1:
- Input: nums = [3,2,3]
- Output: [3]

### Example 2:
- Input: nums = [1]
- Output: [1]

### Example 3:
- Input: nums = [1,2]
- Output: [1,2]

## Constraints
- 1 <= nums.length <= 5 * 10^4
- -10^9 <= nums[i] <= 10^9

## Solution
```java
class Solution {
	public List<Integer> majorityElement(int[] nums) {
		Map<Integer, Integer> map = new HashMap<>();
		for (int num : nums) {
			map.put(num, map.getOrDefault(num, 0) + 1);
		}
        
		List<Integer> ans = new ArrayList<>();
		for (int key : map.keySet()) {
			if (map.get(key) > nums.length / 3) {
				ans.add(key);
			}
		}
		return ans;
	}
}
```

Approach

- Use a HashMap to count the frequency of each element in the array.
- Iterate through the array and update the count for each number.
- After counting, iterate through the map to collect elements that appear more than n/3 times.
- Return the list of such elements.
- Notes: This approach is straightforward but uses extra space for the frequency map.

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(n), for the HashMap storing frequencies.
