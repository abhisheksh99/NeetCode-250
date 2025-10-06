# 128. Longest Consecutive Sequence
**Medium**

## Problem Statement
Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.
You must write an algorithm that runs in O(n) time.

## Examples
### Example 1:
- Input: nums = [100,4,200,1,3,2]
- Output: 4
- Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.

### Example 2:
- Input: nums = [0,3,7,2,5,8,4,6,0,1]
- Output: 9

## Constraints
- 0 <= nums.length <= 10^5
- -10^9 <= nums[i] <= 10^9

## Solution
```java
class Solution {
	public int longestConsecutive(int[] nums) {
		if(nums.length==0){
			return 0;
		}
		HashSet<Integer> numSet = new HashSet<>();
        
		for(int num:nums){
			numSet.add(num);
		}
		int longest=0;
		for(int num:numSet){
			if(!numSet.contains(num-1)){
				int length=1;
				while(numSet.contains(num+length)){
					length++;
				}
				longest=Math.max(longest,length);
			}
		}
		return longest;
	}
}
```

Approach

- Add all numbers to a HashSet for O(1) lookups.
- For each number, check if it is the start of a sequence (i.e., num-1 is not in the set).
- If it is, incrementally check for consecutive numbers (num+1, num+2, ...) and count the sequence length.
- Track the maximum sequence length found.
- Notes: Each number is checked at most twice, ensuring O(n) time.

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(n), for the HashSet storing all unique numbers.
