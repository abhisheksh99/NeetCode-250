# 69. Sqrt(x)
**Easy**

## Problem Statement
Given a non-negative integer `x`, compute and return the square root of `x` rounded down to the nearest integer. The returned integer should be non-negative as well.

You must not use any built-in exponent function or operator.

## Examples
### Example 1:
- Input: x = 4
- Output: 2

### Example 2:
- Input: x = 8
- Output: 2
- Explanation: The square root of 8 is 2.82842..., and since we want the floor, the answer is 2.

## Constraints
- 0 <= x <= 2^31 - 1

## Solution
```java
class Solution {
	public int mySqrt(int x) {
		if (x < 2) return x;  // sqrt(0)=0, sqrt(1)=1

		int left = 1, right = x / 2, result = 0;

		while (left <= right) {
			int mid = left + (right - left) / 2;
			long square = (long) mid * mid;

			if (square == x) {
				return mid;  // perfect square root
			} else if (square < x) {
				result = mid; // potential answer
				left = mid + 1;
			} else {
				right = mid - 1;
			}
		}
		return result;  // floor(sqrt(x))
	}
}
```

Approach

- Use binary search between 1 and x/2 to find the integer square root.
- For each mid, check if mid*mid == x, mid*mid < x, or mid*mid > x.
- If mid*mid == x, return mid.
- If mid*mid < x, move left up and store mid as a potential answer.
- If mid*mid > x, move right down.
- Return the last valid mid as the answer.

Time Complexity

O(log x), since the search space is halved each time.

Space Complexity

O(1), as the search is performed in-place.
