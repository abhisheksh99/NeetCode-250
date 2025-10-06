# 673. Number of Longest Increasing Subsequence

## Problem Statement
Given an integer array `nums`, return the number of longest increasing subsequences.

**Notice** that the sequence has to be strictly increasing.

## Examples

### Example 1:
```
Input: nums = [1,3,5,4,7]
Output: 2
Explanation: The two longest increasing subsequences are [1, 3, 4, 7] and [1, 3, 5, 7].
```

### Example 2:
```
Input: nums = [2,2,2,2,2]
Output: 5
Explanation: The length of longest increasing subsequence is 1, and there are 5 increasing subsequences of length 1, so output 5.
```

## Approach
We can solve this problem using dynamic programming. We'll maintain two arrays:
1. `lengths[i]`: The length of the longest increasing subsequence ending at index `i`.
2. `counts[i]`: The number of longest increasing subsequences ending at index `i` with length `lengths[i]`.

### Steps:
1. Initialize both `lengths` and `counts` arrays with 1, as each element is a valid subsequence of length 1.
2. For each element at index `i`, iterate through all previous elements `j` from `0` to `i-1`:
   - If `nums[j] < nums[i]`, it means we can extend the subsequence ending at `j` to include `nums[i]`.
   - If `lengths[j] + 1 > lengths[i]`, update `lengths[i]` and set `counts[i] = counts[j]`.
   - Else if `lengths[j] + 1 == lengths[i]`, increment `counts[i]` by `counts[j]`.
3. Find the maximum length in `lengths` array.
4. Sum up all `counts[i]` where `lengths[i] == max_length`.

## Solution Code
```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int n = nums.length;
        int[] lengths = new int[n];  // lengths[i] = length of longest ending in nums[i]
        int[] counts = new int[n];   // count[i] = number of longest ending in nums[i]
        Arrays.fill(lengths, 1);
        Arrays.fill(counts, 1);
        
        int maxLength = 1;  // Length of longest increasing subsequence
        
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    if (lengths[j] + 1 > lengths[i]) {
                        // Found a longer subsequence
                        lengths[i] = lengths[j] + 1;
                        counts[i] = counts[j];  // Reset count with new length
                    } else if (lengths[j] + 1 == lengths[i]) {
                        // Found another subsequence with same max length
                        counts[i] += counts[j];
                    }
                }
            }
            maxLength = Math.max(maxLength, lengths[i]);
        }
        
        // Count all sequences with max length
        int result = 0;
        for (int i = 0; i < n; i++) {
            if (lengths[i] == maxLength) {
                result += counts[i];
            }
        }
        
        return result;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n²) - We have nested loops where for each element, we check all previous elements.
- **Space Complexity**: O(n) - We use two additional arrays of size n to store lengths and counts.

## Key Insights
1. **Dynamic Programming**: We use dynamic programming to build up the solution by solving smaller subproblems first.
2. **Two Arrays**: We maintain both the length of the longest subsequence and the count of such subsequences ending at each index.
3. **Efficient Counting**: By keeping track of counts as we build the solution, we avoid having to reconstruct the subsequences.

## Edge Cases
1. **Empty Array**: The input array is empty (should return 0).
2. **Single Element**: The array contains only one element (should return 1).
3. **All Elements Equal**: All elements in the array are the same (should return the length of the array).
4. **Strictly Increasing**: The array is already strictly increasing (should return 1).
5. **Strictly Decreasing**: The array is strictly decreasing (each element is a subsequence of length 1).

## Follow-up
How would you modify the solution if the problem asked for the actual longest increasing subsequences instead of just the count? (This would require additional space to store the sequences.)