# 452. Minimum Number of Arrows to Burst Balloons

## Problem Statement
There are some spherical balloons taped onto a flat wall that represents the XY-plane. The balloons are represented as a 2D integer array `points` where `points[i] = [x_start, x_end]` denotes a balloon whose horizontal diameter stretches between `x_start` and `x_end`. You do not know the exact y-coordinates of the balloons.

Arrows can be shot directly up from different points along the x-axis. A balloon with `x_start` and `x_end` is burst by an arrow shot at `x` if `x_start <= x <= x_end`. There is no limit to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.

Given the array `points`, return the minimum number of arrows that must be shot to burst all balloons.

## Examples

### Example 1:
```
Input: points = [[10,16],[2,8],[1,6],[7,12]]
Output: 2
Explanation: One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons).
```

### Example 2:
```
Input: points = [[1,2],[3,4],[5,6],[7,8]]
Output: 4
Explanation: One arrow needs to be shot for each balloon.
```

### Example 3:
```
Input: points = [[1,2],[2,3],[3,4],[4,5]]
Output: 2
Explanation: The balloons can be burst by 2 arrows:
- Shoot an arrow at x = 2, bursting the first two balloons.
- Shoot an arrow at x = 4, bursting the last two balloons.
```

## Approach
This problem is a classic interval scheduling problem where we want to find the minimum number of points (arrows) that can cover all intervals (balloons). The optimal approach is to use a greedy algorithm:

1. **Sort the intervals**: First, sort the balloons based on their end coordinates. This allows us to process the balloons in a way that maximizes the number of balloons that can be burst with a single arrow.

2. **Greedy selection**: Initialize the end of the first balloon as the position to shoot the first arrow. Then, for each subsequent balloon, if it starts after the current arrow position, we need a new arrow, and we update the current arrow position to the end of this balloon.

### Key Insight
- By sorting the intervals by their end points, we ensure that we're always making the optimal greedy choice at each step.
- The idea is that if we shoot an arrow at the end of the first balloon, we can burst all overlapping balloons that start before this end point.

## Solution Code
```java
import java.util.Arrays;
import java.util.Comparator;

class Solution {
    public int findMinArrowShots(int[][] points) {
        if (points.length == 0) {
            return 0;
        }
        
        // Sort the balloons by their end coordinates
        Arrays.sort(points, Comparator.comparingInt(a -> a[1]));
        
        int arrows = 1;
        int arrowPos = points[0][1];
        
        for (int i = 1; i < points.length; i++) {
            // If the current balloon starts after the arrow position, we need a new arrow
            if (points[i][0] > arrowPos) {
                arrows++;
                arrowPos = points[i][1];
            }
            // Else, the current balloon can be burst by the current arrow
        }
        
        return arrows;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n log n) - The dominant factor is the sorting step. The subsequent iteration through the sorted array is O(n).
- **Space Complexity**: O(log n) - The space used by the sorting algorithm (for the recursive call stack in the default Java sorting algorithm). The rest of the solution uses constant extra space.

## Key Insights
1. **Greedy Choice**: The optimal strategy is to place the arrow at the end of the first balloon's interval, as this allows us to cover as many overlapping balloons as possible.
2. **Sorting by End Points**: Sorting by the end points ensures that we can make the greedy choice at each step without needing to backtrack.
3. **Efficiency**: The greedy approach ensures that we find the minimum number of arrows needed in optimal time.

## Edge Cases
1. **Empty Input**: If there are no balloons, return 0.
2. **Single Balloon**: If there's only one balloon, we need exactly one arrow.
3. **Non-overlapping Balloons**: Each balloon requires its own arrow if none overlap.
4. **Completely Overlapping Balloons**: If all balloons overlap at a single point, only one arrow is needed.
5. **Large Input**: The solution should handle the maximum constraints efficiently (up to 10^5 balloons).

## Follow-up
How would you modify the solution if the arrows could be shot at any angle (not just vertically)? (This would require a different approach, possibly involving computational geometry concepts like line segment intersection.)