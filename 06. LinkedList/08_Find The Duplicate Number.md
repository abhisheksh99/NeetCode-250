# 287. Find the Duplicate Number
**Medium**

## Problem Statement
Given an array of integers nums containing n + 1 integers where each integer is in the range [1, n] inclusive.

There is only one repeated number in nums, return this repeated number.

You must solve the problem without modifying the array nums and uses only constant extra space.

## Examples
### Example 1:
- Input: nums = [1,3,4,2,2]
- Output: 2

### Example 2:
- Input: nums = [3,1,3,4,2]
- Output: 3

## Constraints
- 1 <= n <= 10^5
- nums.length == n + 1
- 1 <= nums[i] <= n
- All the integers in nums appear only once except for precisely one integer which appears two or more times.
- You must solve the problem without modifying the array nums and uses only constant extra space.

## Solution
```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = nums[0];
        int fast = nums[nums[0]];
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[nums[fast]];
        }
        slow = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
}
```

Approach
- Use Floyd's Tortoise and Hare (cycle detection) algorithm.
- Treat the array as a linked list where the value at each index points to the next index.
- The duplicate number forms a cycle in this "linked list".
- First, find the intersection point of the two runners (slow and fast).
- Then, reset one pointer to the start and move both one step at a time to find the entrance to the cycle (duplicate number).

Time Complexity
O(n), where n is the length of the array.

Space Complexity
O(1), constant extra space.
