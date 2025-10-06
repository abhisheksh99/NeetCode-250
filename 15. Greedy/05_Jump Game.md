# 55. Jump Game
**Medium**

## Problem Statement
You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

## Examples
### Example 1:
- Input: `nums = [2,3,1,1,4]`
- Output: `true`
- Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

### Example 2:
- Input: `nums = [3,2,1,0,4]`
- Output: `false`
- Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

## Constraints
- `1 <= nums.length <= 10^4`
- `0 <= nums[i] <= 10^5`

## Solution
```java
class Solution {
    public boolean canJump(int[] nums) {
        int goal = nums.length-1;

        for(int i = nums.length-2; i >= 0; i--) {
            if(i + nums[i] >= goal) {
                goal = i;
            }
        }
        return goal == 0;
    }
}
```

## Approach
1. **Greedy Backward Traversal**:
   - Start from the end of the array and work backwards.
   - Maintain a `goal` variable that represents the current target index we're trying to reach.
   - For each index `i`, check if we can jump from `i` to the current `goal`.
   - If yes, update the `goal` to `i`.
   - Finally, check if we've reached the start of the array (`goal == 0`).

2. **Key Insight**:
   - If we can reach the current `goal` from index `i`, then we only need to check if we can reach `i` from the start.
   - This approach efficiently reduces the problem size at each step.

## Time Complexity
- **O(n)**: We make a single pass through the array from the second-to-last element to the first.

## Space Complexity
- **O(1)**: We use a constant amount of extra space (just the `goal` variable).

## Edge Cases
- Single element array: Always returns `true` (already at the end).
- Array contains a `0` that can't be jumped over.
- Array where all elements are large enough to jump to the end in one step.
- Large input size: Handles up to 10,000 elements efficiently.
- Large element values: Handles values up to 100,000.