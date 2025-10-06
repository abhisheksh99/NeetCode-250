# 918. Maximum Sum Circular Subarray
**Medium**

## Problem Statement
Given a circular integer array `nums` of length `n`, return the maximum possible sum of a non-empty subarray of `nums`.

A circular array means the end of the array connects to the beginning of the array. Formally, the next element of `nums[i]` is `nums[(i + 1) % n]` and the previous element of `nums[i]` is `nums[(i - 1 + n) % n]`.

## Examples
### Example 1:
- Input: `nums = [1,-2,3,-2]`
- Output: `3`
- Explanation: Subarray `[3]` has the maximum sum 3.

### Example 2:
- Input: `nums = [5,-3,5]`
- Output: `10`
- Explanation: Subarray `[5,5]` (formed by the first and last element) has the maximum sum 10.

### Example 3:
- Input: `nums = [-3,-2,-3]`
- Output: `-2`
- Explanation: Subarray `[-2]` has the maximum sum -2.