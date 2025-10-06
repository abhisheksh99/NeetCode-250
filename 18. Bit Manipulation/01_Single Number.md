# 136. Single Number
**Easy**

## Problem Statement
Given a non-empty array of integers `nums`, every element appears twice except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

## Examples
### Example 1:
- Input: nums = [2,2,1]
- Output: 1

### Example 2:
- Input: nums = [4,1,2,1,2]
- Output: 4

### Example 3:
- Input: nums = [1]
- Output: 1

## Constraints
- 1 <= nums.length <= 3 * 10^4
- -3 * 10^4 <= nums[i] <= 3 * 10^4
- Each element in the array appears twice except for one element which appears only once.

## Solution
```java
public class Solution {
    public int singleNumber(int[] nums) {
        int res = 0;
        for (int num : nums) {
            res ^= num;
        }
        return res;
    }
}
```

## Approach

- Use the XOR bitwise operator to find the single number that appears only once.
- Initialize a result variable to 0.
- Iterate through each number in the array and XOR it with the result.
- XOR has the property that a ^ a = 0 and a ^ 0 = a, so all paired numbers will cancel out to 0.
- The only number left after all XOR operations is the single number that appears once.
- Return the final result.

## Time Complexity

O(n), where n is the length of the array, as we iterate through the array once.

## Space Complexity

O(1), as we only use a single variable to store the result.