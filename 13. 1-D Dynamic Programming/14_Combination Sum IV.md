# 377. Combination Sum IV

## Problem Statement
Given an array of distinct integers `nums` and a target integer `target`, return the number of possible combinations that add up to `target`.

The test cases are generated so that the answer can fit in a 32-bit integer.

### Examples

**Example 1:**
```
Input: nums = [1,2,3], target = 4
Output: 7
Explanation:
The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
Note that different sequences are counted as different combinations.
```

**Example 2:**
```
Input: nums = [9], target = 3
Output: 0
```

### Constraints:
- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 1000`
- All the elements of `nums` are unique.
- `1 <= target <= 1000`

## Solution
```java
public class Solution {
    public int combinationSum4(int[] nums, int target) {
        Arrays.sort(nums);
        Map<Integer, Integer> dp = new HashMap<>();
        dp.put(target, 1);
        
        for (int total = target; total > 0; total--) {
            for (int num : nums) {
                if (total < num) break;
                dp.put(total - num, dp.getOrDefault(total, 0) + dp.getOrDefault(total - num, 0));
            }
        }
        
        return dp.getOrDefault(0, 0);
    }
}
```

## Approach
1. **Sorting and Initialization**:
   - Sort the input array to enable early termination in the inner loop.
   - Use a `HashMap` to store the number of ways to reach each sum.
   - Initialize the target sum with 1 way (the empty combination).

2. **Dynamic Programming Update**:
   - Iterate from the target down to 0.
   - For each number in the sorted array, update the number of ways to reach `(current_sum - num)`.
   - If the current number is greater than the remaining target, break the inner loop (optimization).

3. **Result Extraction**:
   - The value at key `0` in the map represents the number of ways to reach the target sum.

## Complexity
- **Time Complexity**: O(n * target)
  - Where `n` is the number of elements in `nums` and `target` is the target sum.
  - In the worst case, we might process each number for each possible sum up to the target.

- **Space Complexity**: O(target)
  - The `HashMap` can store up to `target` different sums.

## Edge Cases
- `nums = [1,2,3], target = 4` → `7` (as shown in example 1)
- `nums = [9], target = 3` → `0` (no combination possible)
- `nums = [1], target = 1` → `1` (only one way: [1])
- `nums = [1,2], target = 3` → `3` ([1,1,1], [1,2], [2,1])

## Key Insights
- The problem is a variation of the unbounded knapsack problem.
- The solution uses a top-down DP approach with memoization.
- Sorting the array allows for early termination in the inner loop when the remaining numbers are too large.
- The order of combinations matters (unlike in the standard combination sum problem).
- The solution efficiently counts all possible combinations without explicitly generating them, which is crucial for larger inputs.