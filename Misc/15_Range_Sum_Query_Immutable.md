# 303. Range Sum Query - Immutable

## Problem Statement
Given an integer array `nums`, handle multiple queries of the following type:

1. `sumRange(left, right)`: Calculate the sum of the elements of `nums` between indices `left` and `right` inclusive.

Implement the `NumArray` class:
- `NumArray(int[] nums)` Initializes the object with the integer array `nums`.
- `int sumRange(int left, int right)` Returns the sum of the elements of `nums` between indices `left` and `right` inclusive (i.e., `nums[left] + nums[left + 1] + ... + nums[right]`).

## Examples

### Example 1:
```
Input
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
Output
[null, 1, -1, -3]

Explanation
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3
```

## Approach
We can optimize the sum range queries by precomputing the prefix sums of the array. The prefix sum at index `i` is the sum of all elements from the start of the array up to (but not including) index `i`.

1. **Prefix Sum Array**: Create an array `prefix` where `prefix[i]` is the sum of all elements before index `i`.
2. **Sum Calculation**: The sum of elements from index `left` to `right` can be calculated as `prefix[right + 1] - prefix[left]`.

This approach allows us to answer each query in O(1) time after O(n) preprocessing.

## Solution Code
```java
class NumArray {
    private int[] prefixSum;

    public NumArray(int[] nums) {
        int n = nums.length;
        prefixSum = new int[n + 1];
        
        // Calculate prefix sum
        for (int i = 0; i < n; i++) {
            prefixSum[i + 1] = prefixSum[i] + nums[i];
        }
    }
    
    public int sumRange(int left, int right) {
        // The sum of elements from index left to right is prefixSum[right + 1] - prefixSum[left]
        return prefixSum[right + 1] - prefixSum[left];
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(left,right);
 */
```

## Complexity Analysis
- **Time Complexity**:
  - **Constructor**: O(n) - We iterate through the input array once to compute the prefix sums.
  - **sumRange**: O(1) - Each query is answered in constant time.
- **Space Complexity**: O(n) - We use an additional array of size n+1 to store the prefix sums.

## Key Insights
1. **Prefix Sum Array**: The prefix sum array allows us to compute the sum of any subarray in constant time.
2. **Efficient Queries**: After the initial O(n) preprocessing, each query is answered in O(1) time, making this approach efficient for a large number of queries.
3. **Inclusive/Exclusive Bounds**: The prefix sum array is defined such that `prefix[i]` is the sum of elements before index `i`, making the range sum calculation straightforward.

## Edge Cases
1. **Empty Array**: The input array can be empty, but the problem constraints state that `1 <= nums.length <= 10^4`.
2. **Single Element**: The array contains only one element, and the query is for that single element.
3. **Full Range**: The query covers the entire array.
4. **Large Input**: The array contains the maximum number of elements (10^4), and we need to handle multiple queries efficiently.

## Follow-up
How would you modify the solution if the array could be updated between queries? (This would require a different approach, such as using a Binary Indexed Tree or a Segment Tree.)