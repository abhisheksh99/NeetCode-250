# 42. Trapping Rain Water
**Hard**

## Problem Statement
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

## Examples
### Example 1:
- Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
- Output: 6

### Example 2:
- Input: height = [4,2,0,3,2,5]
- Output: 9

## Constraints
- n == height.length
- 1 <= n <= 2 * 10^4
- 0 <= height[i] <= 10^5

## Solution
```java
class Solution {
	public int trap(int[] height) {
		int left = 0, right = height.length - 1;
		int leftMax = 0, rightMax = 0;
		int water = 0;

		while (left <= right) {
			if (height[left] <= height[right]) {
				if (height[left] >= leftMax) {
					leftMax = height[left]; // update left max
				} else {
					water += leftMax - height[left]; // trapped water at left
				}
				left++;
			} else {
				if (height[right] >= rightMax) {
					rightMax = height[right]; // update right max
				} else {
					water += rightMax - height[right]; // trapped water at right
				}
				right--;
			}
		}

		return water;
	}
}
```

Approach

- Use two pointers, `left` and `right`, at the start and end of the array.
- Track the maximum height seen so far from the left (`leftMax`) and right (`rightMax`).
- At each step, move the pointer with the smaller height inward.
- If the current height is less than the max seen so far, add the difference to the water trapped.
- Continue until the pointers meet.
- Notes: This approach efficiently computes trapped water in a single pass.

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(1), as only a few pointers/variables are used.
