# 45. Jump Game II
**Medium**

## Problem Statement
Given an array of non-negative integers `nums`, you are initially positioned at the first index of the array. Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps. You can assume that you can always reach the last index.

## Examples
### Example 1:
- Input: `nums = [2,3,1,1,4]`
- Output: `2`
- Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

### Example 2:
- Input: `nums = [2,3,0,1,4]`
- Output: `2`
- Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

## Constraints
- `1 <= nums.length <= 10^4`
- `0 <= nums[i] <= 1000`

## Solution
```java
class Solution {
    public int jump(int[] nums) {
        int jump = 0, currMax = 0, currEnd = 0;
        
        for (int i = 0; i < nums.length - 1; i++) {
            currMax = Math.max(currMax, i + nums[i]);
            if (i == currEnd) {
                jump++;
                currEnd = currMax;
            }
        }
        
        return jump;
    }
}
```

## Approach
1. **Greedy BFS-like Approach**:
   - Use three variables:
     - `jump`: Counts the number of jumps made
     - `currMax`: Tracks the farthest index that can be reached in the current jump
     - `currEnd`: Marks the end of the current jump's range
   - For each index, update `currMax` to be the maximum of its current value and the farthest we can jump from the current position
   - When we reach `currEnd`, it means we've considered all positions in the current jump range, so we increment the jump count and update `currEnd` to `currMax`

2. **Key Insight**:
   - Each jump should take us to the farthest possible position that can be reached in the current range
   - This approach ensures we make the optimal number of jumps by always considering the maximum reach from any position in the current range

## Time Complexity
- **O(n)**: We make a single pass through the array, performing constant work for each element.

## Space Complexity
- **O(1)**: We use a constant amount of extra space (just three integer variables).

## Edge Cases
- Single element array: Returns 0 (already at the end)
- Array where we can reach the end in one jump
- Array where we need to make n-1 jumps (each element is 1)
- Large input size: Handles up to 10,000 elements efficiently
- Maximum jump length: Handles the maximum value of 1000