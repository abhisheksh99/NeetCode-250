# 978. Longest Turbulent Subarray
**Medium**

## Problem Statement
Given an integer array `arr`, return the length of a maximum size turbulent subarray of `arr`. A subarray is turbulent if the comparison sign flips between each adjacent pair of elements in the subarray.

More formally, a subarray `[arr[i], arr[i + 1], ..., arr[j]]` of `arr` is said to be turbulent if and only if:
- For `i <= k < j`:
  - `arr[k] > arr[k + 1]` when `k` is odd, and
  - `arr[k] < arr[k + 1]` when `k` is even,
- Or, for `i <= k < j`:
  - `arr[k] > arr[k + 1]` when `k` is even, and
  - `arr[k] < arr[k + 1]` when `k` is odd.

## Examples
### Example 1:
- Input: `arr = [9,4,2,10,7,8,8,1,9]`
- Output: `5`
- Explanation: `arr[1] > arr[2] < arr[3] > arr[4] < arr[5]`

### Example 2:
- Input: `arr = [4,8,12,16]`
- Output: `2`
- Explanation: `[4,8]` or `[8,12]` or `[12,16]` are all possible maximum turbulent subarrays.

### Example 3:
- Input: `arr = [100]`
- Output: `1`

## Constraints
- `1 <= arr.length <= 4 * 10^4`
- `0 <= arr[i] <= 10^9`

## Solution
```java
class Solution {
    public int maxTurbulenceSize(int[] arr) {
        int n = arr.length;
        if (n < 2) return n;
        
        int maxLength = 1;
        int up = 1, down = 1;
        
        for (int i = 1; i < n; i++) {
            if (arr[i] > arr[i - 1]) {
                up = down + 1;
                down = 1;
            } else if (arr[i] < arr[i - 1]) {
                down = up + 1;
                up = 1;
            } else {
                up = 1;
                down = 1;
            }
            maxLength = Math.max(maxLength, Math.max(up, down));
        }
        
        return maxLength;
    }
}
```

## Approach
1. **Dynamic Programming with State Tracking**:
   - We maintain two counters: `up` and `down`
   - `up` tracks the length of the longest turbulent subarray ending at current index where the last comparison was `arr[i-1] < arr[i]`
   - `down` tracks the length of the longest turbulent subarray ending at current index where the last comparison was `arr[i-1] > arr[i]`

2. **State Transitions**:
   - If `arr[i] > arr[i-1]`, then `up = down + 1` (since we're continuing a down sequence)
   - If `arr[i] < arr[i-1]`, then `down = up + 1` (since we're continuing an up sequence)
   - If `arr[i] == arr[i-1]`, both sequences reset to 1

3. **Result Calculation**:
   - After processing each element, we update the maximum length found so far

## Time Complexity
- **O(n)**: We make a single pass through the array, performing constant work for each element.

## Space Complexity
- **O(1)**: We use a constant amount of extra space for variables.

## Edge Cases
- Single element array: Returns 1
- All elements equal: Returns 1
- Strictly increasing or decreasing array: Returns 2
- Large input size: Handles up to 40,000 elements efficiently
- Large element values: Handles values up to 10^9