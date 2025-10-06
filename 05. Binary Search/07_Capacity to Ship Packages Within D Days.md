# 1011. Capacity To Ship Packages Within D Days
**Medium**

## Problem Statement
Given an array `weights` representing the weights of packages and an integer `days`, return the least weight capacity of a ship that will result in all the packages being shipped within the given number of days.

Each day, the ship loads packages in the order given by `weights`. The ship will not load more weight than its capacity. Once the ship starts loading packages for the day, it cannot stop until it has loaded all the packages for that day.

## Examples
### Example 1:
- Input: weights = [1,2,3,4,5,6,7,8,9,10], days = 5
- Output: 15

### Example 2:
- Input: weights = [3,2,2,4,1,4], days = 3
- Output: 6

### Example 3:
- Input: weights = [1,2,3,1,1], days = 4
- Output: 3

## Constraints
- 1 <= days <= weights.length <= 5 * 10^4
- 1 <= weights[i] <= 500

## Solution
```java
class Solution {
	private boolean canShip(int[] weights, int capacity, int days) {
		int dayCount = 1, load = 0;
		for (int weight : weights) {
			load += weight;
			if (load > capacity) {
				dayCount++;
				load = weight;
				if (dayCount > days) return false;
			}
		}
		return true;
	}

	public int shipWithinDays(int[] weights, int days) {
		int maxWeight = 0, totalWeight = 0;
		for (int weight : weights) {
			maxWeight = Math.max(maxWeight, weight);
			totalWeight += weight;
		}

		int left = maxWeight, right = totalWeight;
        
		while (left < right) {
			int mid = left + (right - left) / 2;
			if (canShip(weights, mid, days)) {
				right = mid;
			} else {
				left = mid + 1;
			}
		}
		return left;
	}
}
```

Approach

- Use binary search on the answer (minimum ship capacity).
- The search range is from the maximum single package weight to the sum of all weights.
- For each candidate capacity, check if it's possible to ship all packages within the given days using a helper function.
- If possible, try a smaller capacity; otherwise, increase the capacity.
- Return the minimum capacity found.

Time Complexity

O(n log S), where n is the number of packages and S is the sum of all weights.

Space Complexity

O(1), as only a few variables are used.
