# Car Pooling

**Difficulty:** Medium

## Problem Statement
You are given an array `trips` where `trips[i] = [numPassengers, from, to]` indicates that the i-th trip has `numPassengers` passengers, the car will pick up the passengers at location `from` and drop them off at location `to`. You are also given an integer `capacity` representing the maximum number of passengers the car can hold.

Return true if it is possible to pick up and drop off all passengers for all the given trips without exceeding the capacity, or false otherwise.

**Example:**
```
Input: trips = [[2,1,5],[3,3,7]], capacity = 4
Output: false

Input: trips = [[2,1,5],[3,3,7]], capacity = 5
Output: true
```

**Constraints:**
- 1 <= trips.length <= 1000
- 1 <= numPassengers <= 100
- 0 <= from < to <= 1000
- 1 <= capacity <= 10^5

---

## Java Solution
```java
class Solution {
	public boolean carPooling(int[][] trips, int capacity) {
		int[] stops = new int[1001]; // Since 0 <= from, to <= 1000

		// Step 1: Mark the passenger changes
		for (int[] trip : trips) {
			int num = trip[0];
			int from = trip[1];
			int to = trip[2];

			stops[from] += num; // passengers get in
			stops[to] -= num;   // passengers get out
		}

		// Step 2: Simulate the journey
		int currPassengers = 0;
		for (int i = 0; i < 1001; i++) {
			currPassengers += stops[i];
			if (currPassengers > capacity) return false;
		}

		return true;
	}
}
```

---

## Approach
- Use a difference array to track the number of passengers getting in and out at each location.
- For each trip, increment at the start location and decrement at the end location.
- Simulate the journey by accumulating the passenger count at each stop.
- If at any point the number of passengers exceeds the car's capacity, return false.

---

## Time and Space Complexity
- **Time Complexity:** O(N + R), where N is the number of trips and R is the range of locations (up to 1001).
- **Space Complexity:** O(R) for the stops array.
