# 300. Longest Increasing Subsequence

## Problem Statement
Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A subsequence is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements.

### Examples

**Example 1:**
```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

**Example 2:**
```
Input: nums = [0,1,0,3,2,3]
Output: 4
```

**Example 3:**
```
Input: nums = [7,7,7,7,7,7,7]
Output: 1
```

### Constraints:
- `1 <= nums.length <= 2500`
- `-10^4 <= nums[i] <= 10^4`

## Solution
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] Lis = new int[nums.length];
        Arrays.fill(Lis, 1);
        int max = 1;
        
        for(int i = 1; i < nums.length; i++) {
            for(int j = 0; j < i; j++) {
                if(nums[i] > nums[j]) {
                    Lis[i] = Math.max(Lis[i], 1 + Lis[j]);
                    max = Math.max(max, Lis[i]);
                }
            }
        }
        
        return max;
    }
}
```

## Approach
1. **Dynamic Programming**:
   - We use an array `Lis` where `Lis[i]` represents the length of the longest increasing subsequence ending at index `i`.
   - Initialize all values in `Lis` to 1 since each element is a subsequence of length 1.
   - For each element at index `i`, compare it with all previous elements (from index 0 to i-1).
   - If `nums[i]` is greater than `nums[j]`, update `Lis[i]` to be the maximum of its current value and `1 + Lis[j]`.
   - Keep track of the maximum length found so far in the `max` variable.
   - Finally, return `max` which holds the length of the longest increasing subsequence.

## Complexity
- **Time Complexity**: O(n²)
  - We have nested loops where for each element, we compare it with all previous elements.
- **Space Complexity**: O(n)
  - We use an additional array `Lis` of size n to store the length of the longest increasing subsequence ending at each index.

## Edge Cases
- `nums = [1]` → `1` (single element)
- `nums = [5,4,3,2,1]` → `1` (strictly decreasing)
- `nums = [1,2,3,4,5]` → `5` (strictly increasing)
- `nums = [10,9,2,5,3,7,101,18]` → `4` (example case)

## Key Insights
- The key insight is recognizing that the problem has overlapping subproblems and optimal substructure, making it suitable for dynamic programming.
- By solving smaller subproblems first (longest increasing subsequence ending at each index), we can build up the solution to the larger problem.
- The solution efficiently computes the length of the longest increasing subsequence by leveraging previously computed results for smaller subarrays.