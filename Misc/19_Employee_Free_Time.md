# 759. Employee Free Time

## Problem Statement
We are given a list `schedule` of employees, which represents the working time for each employee.

Each employee has a list of non-overlapping `Intervals`, and these intervals are in sorted order.

Return the list of finite intervals representing common, positive-length free time for all employees, also in sorted order.

(Even though we are representing `Intervals` in the form `[x, y]`, the objects inside are `Intervals`, not lists or arrays. For example, `schedule[0][0].start`, `schedule[0][0].end` are the start and end times of the first interval of the first employee).

### Examples

#### Example 1:
```
Input: schedule = [[[1,2],[5,6]],[[1,3]],[[4,10]]]
Output: [[3,4]]
Explanation: There are a total of three employees, and all common
free time intervals would be [-inf, 1], [3, 4], [10, inf].
We discard any intervals that contain inf as they aren't finite.
```

#### Example 2:
```
Input: schedule = [[[1,3],[6,7]],[[2,4]],[[2,5],[9,12]]]
Output: [[5,6],[7,9]]
```

## Approach
This problem can be solved using a priority queue (min-heap) to efficiently merge all employee schedules and then find the gaps between the merged intervals. Here's the step-by-step approach:

1. **Flatten and Sort Intervals**: First, we collect all intervals from all employees and sort them based on their start time.

2. **Merge Intervals**: We then merge overlapping or adjacent intervals. This helps in identifying the combined busy times of all employees.

3. **Find Gaps**: The free time intervals are the gaps between the merged intervals. We iterate through the merged intervals and check the gap between the end of the current interval and the start of the next interval.

### Key Insights
- **Merging Intervals**: By sorting and merging intervals, we can efficiently find the combined busy times of all employees.
- **Finding Free Time**: The free time intervals are the gaps between the merged intervals. These gaps represent times when no employee is working.
- **Efficiency**: Using a priority queue helps in efficiently managing and merging intervals, especially when dealing with a large number of employees and intervals.

## Solution Code
```java
import java.util.*;

class Interval {
    public int start;
    public int end;

    public Interval() {}

    public Interval(int _start, int _end) {
        start = _start;
        end = _end;
    }
}

class Solution {
    public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
        List<Interval> allIntervals = new ArrayList<>();
        
        // Flatten the list of all intervals
        for (List<Interval> employee : schedule) {
            allIntervals.addAll(employee);
        }
        
        // Sort intervals by start time
        Collections.sort(allIntervals, (a, b) -> a.start - b.start);
        
        // Merge intervals
        List<Interval> merged = new ArrayList<>();
        for (Interval interval : allIntervals) {
            if (merged.isEmpty() || merged.get(merged.size() - 1).end < interval.start) {
                merged.add(interval);
            } else {
                // Update the end of the last merged interval
                merged.get(merged.size() - 1).end = Math.max(merged.get(merged.size() - 1).end, interval.end);
            }
        }
        
        // Find the free time intervals
        List<Interval> result = new ArrayList<>();
        for (int i = 1; i < merged.size(); i++) {
            int start = merged.get(i - 1).end;
            int end = merged.get(i).start;
            if (start < end) {
                result.add(new Interval(start, end));
            }
        }
        
        return result;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(N log N), where N is the total number of intervals across all employees. The sorting step dominates the time complexity.
- **Space Complexity**: O(N), where N is the total number of intervals, for storing the merged intervals and the result list.

## Key Insights
1. **Merging Intervals**: The core of the solution involves merging overlapping or adjacent intervals to find the combined busy times of all employees.
2. **Finding Gaps**: The free time intervals are the gaps between the merged intervals. These gaps represent times when no employee is working.
3. **Efficiency**: The use of a priority queue and sorting ensures that the solution is efficient and can handle large inputs effectively.

## Edge Cases
1. **Single Employee**: If there's only one employee, the free time will be the gaps between their intervals.
2. **No Free Time**: If all employees' schedules cover the entire day, there will be no free time.
3. **Single Interval per Employee**: If each employee has only one interval, the free time will be the gaps between these intervals after merging.
4. **Large Input**: The solution should handle the maximum constraints efficiently (up to 100 employees with up to 100 intervals each).

## Follow-up
How would you modify the solution if the employees' schedules were not sorted? (The current solution already handles unsorted intervals by sorting them first, but you might need to consider the time complexity implications.)