# 875. Koko Eating Bananas
**Medium**

## Problem Statement
Koko loves to eat bananas. There are `n` piles of bananas, the i-th pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses a pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Return the minimum integer `k` such that she can eat all the bananas within `h` hours.

## Examples
### Example 1:
- Input: piles = [3,6,7,11], h = 8
- Output: 4

### Example 2:
- Input: piles = [30,11,23,4,20], h = 5
- Output: 30

### Example 3:
- Input: piles = [30,11,23,4,20], h = 6
- Output: 23

## Constraints
- 1 <= piles.length <= 10^4
- piles.length <= h <= 10^9
- 1 <= piles[i] <= 10^9

## Solution
```java
class Solution {
   public static int minEatingSpeed(int[] piles, int h) {
		int left = 1, right = 1;
		for (int pile : piles) {
			right = Math.max(right, pile);
		}

		while (left < right) {
			int mid = left + (right - left) / 2;
			if (canFinish(piles, mid, h)) {
				right = mid;
			} else {
				left = mid + 1;
			}
		}
		return left;
	}

	private static boolean canFinish(int[] piles, int speed, int h) {
		int hours = 0;
		for (int pile : piles) {
			hours += Math.ceil((double) pile / speed);
		}
		return hours <= h;
	}
}
```

Approach

- Use binary search on the answer (minimum eating speed).
- Set the search range from 1 to the maximum pile size.
- For each mid (candidate speed), check if Koko can finish all piles within h hours using a helper function.
- If she can, try a smaller speed; otherwise, increase the speed.
- Return the minimum speed found.

Time Complexity

O(n log m), where n is the number of piles and m is the maximum pile size.

Space Complexity

O(1), as only a few variables are used.
