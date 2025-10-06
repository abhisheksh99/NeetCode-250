# 134. Gas Station
**Medium**

## Problem Statement
There are `n` gas stations along a circular route, where the amount of gas at the `i-th` station is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from the `i-th` station to the next `(i + 1)-th` station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays `gas` and `cost`, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return `-1`. If there exists a solution, it is guaranteed to be unique.

## Examples
### Example 1:
- Input: `gas = [1,2,3,4,5]`, `cost = [3,4,5,1,2]`
- Output: `3`
- Explanation:
  - Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
  - Travel to station 4. Your tank = 4 - 1 + 5 = 8
  - Travel to station 0. Your tank = 8 - 2 + 1 = 7
  - Travel to station 1. Your tank = 7 - 3 + 2 = 6
  - Travel to station 2. Your tank = 6 - 4 + 3 = 5
  - Travel to station 3. The cost is 5. Your tank = 5 - 5 = 0
  - You can travel around the circuit once.

### Example 2:
- Input: `gas = [2,3,4]`, `cost = [3,4,3]`
- Output: `-1`
- Explanation: You can't start at station 0 or 1, as there is not enough gas to travel to the next station.

## Constraints
- `n == gas.length == cost.length`
- `1 <= n <= 10^5`
- `0 <= gas[i], cost[i] <= 10^4`

## Solution
```java
public class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        // If total gas is less than total cost, no solution exists
        if (Arrays.stream(gas).sum() < Arrays.stream(cost).sum()) {
            return -1;
        }

        int total = 0;  // Tracks current gas in tank
        int res = 0;     // Starting station candidate

        for (int i = 0; i < gas.length; i++) {
            total += (gas[i] - cost[i]);

            // If we can't reach the next station from current starting point
            if (total < 0) {
                total = 0;  // Reset gas tank
                res = i + 1; // Try next station as starting point
            }
        }

        return res;
    }
}
```

## Approach
1. **Greedy Algorithm with Early Termination**:
   - First, check if the total gas is at least the total cost. If not, return -1 immediately as no solution exists.
   - Use a greedy approach to find the starting station:
     - Track the current gas in the tank (`total`).
     - Track a candidate starting station (`res`).
     - Iterate through each station, updating the gas in the tank.
     - If the gas becomes negative, reset the tank and try the next station as the starting point.

2. **Key Insights**:
   - If the total gas is less than the total cost, it's impossible to complete the circuit.
   - If we start at station `i` and run out of gas at station `j`, then any station between `i` and `j` cannot be a valid starting point.
   - If we can complete the circuit from station `i` to the end, and we've already checked that total gas >= total cost, then we can complete the circuit.

## Time Complexity
- **O(n)**: We make a single pass through the array, and each operation inside the loop is O(1).

## Space Complexity
- **O(1)**: We use a constant amount of extra space (just a few integer variables).

## Edge Cases
- Single station with enough gas to complete the circuit
- No valid starting station (total gas < total cost)
- Multiple stations where the only valid starting point is the last one
- Large input size (10^5 stations)
- All stations have the same gas and cost values