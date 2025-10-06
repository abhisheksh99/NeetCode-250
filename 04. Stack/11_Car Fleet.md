# 853. Car Fleet
**Medium**

## Problem Statement
There are `n` cars going to the same destination along a one-lane road. The destination is `target` miles away.

You are given two integer arrays `position` and `speed`, both of length `n`, where `position[i]` is the position of the i-th car and `speed[i]` is the speed of the i-th car (all cars are driving towards the target).

A car can never pass another car ahead of it, but it can catch up to it and form a car fleet. A car fleet is some non-empty set of cars driving at the same position and speed. Return the number of car fleets that will arrive at the destination.

## Examples
### Example 1:
- Input: target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3]
- Output: 3

### Example 2:
- Input: target = 10, position = [3], speed = [3]
- Output: 1

## Constraints
- n == position.length == speed.length
- 1 <= n <= 10^5
- 0 < target <= 10^6
- 0 <= position[i] < target
- All the values of position are unique.
- 0 < speed[i] <= 10^6

## Solution
```java
public class Solution {
	public int carFleet(int target, int[] position, int[] speed) {
		int n = position.length;
		double[][] cars = new double[n][2];
        
		for (int i = 0; i < n; i++) {
			cars[i][0] = position[i];
			cars[i][1] = (double)(target - position[i]) / speed[i];
		}
        
		Arrays.sort(cars, (a, b) -> Double.compare(b[0], a[0]));
        
		int count = 0;
		double prevTime = 0;
		for (double[] car : cars) {
			if (car[1] > prevTime) {
				count++;
				prevTime = car[1];
			}
		}
        
		return count;
	}
}
```

Approach

- Pair each car's position with the time it takes to reach the target.
- Sort cars by starting position in descending order (closest to target first).
- Iterate through the sorted cars:
  - If a car's time to reach the target is greater than the previous car's, it forms a new fleet.
  - Otherwise, it joins the previous fleet.
- Count the number of fleets formed.

Time Complexity

O(n log n), due to sorting the cars by position.

Space Complexity

O(n), for storing car positions and times.
