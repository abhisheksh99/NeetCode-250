# 11. Container With Most Water
**Medium**

## Problem Statement
Given n non-negative integers `heights` where each represents a point at coordinate (i, heights[i]), n vertical lines are drawn such that the two endpoints of the line i is at (i, heights[i]) and (i, 0). Find two lines, which together with the x-axis forms a container, such that the container contains the most water.
Return the maximum amount of water a container can store.

## Examples
### Example 1:
- Input: heights = [1,8,6,2,5,4,8,3,7]
- Output: 49

### Example 2:
- Input: heights = [1,1]
- Output: 1

## Constraints
- n == heights.length
- 2 <= n <= 10^5
- 0 <= heights[i] <= 10^4

## Solution
```java
class Solution {
	public int maxArea(int[] heights) {
		int left=0,right=heights.length-1;
		int maxA =0;

		while(left<right){
			int area=Math.min(heights[left],heights[right])*(right-left);
			maxA=Math.max(maxA,area);

			if(heights[left]<heights[right]){
				left++;
			}else{
				right--;
			}
		}
		return maxA;
	}
}
```

Approach

- Use two pointers, `left` at the start and `right` at the end of the array.
- Calculate the area formed by the lines at `left` and `right` and update the maximum area found.
- Move the pointer pointing to the shorter line inward, as this may increase the area.
- Repeat until the pointers meet.
- Notes: This approach efficiently finds the maximum area in a single pass.

Time Complexity

O(n), where n is the length of the array.

Space Complexity

O(1), as only a few pointers/variables are used.
