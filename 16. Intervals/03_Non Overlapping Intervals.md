# 435. Non-overlapping Intervals
**Medium**

## Problem Statement
Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

## Examples
### Example 1:
- Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
- Output: 1
- Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.

### Example 2:
- Input: intervals = [[1,2],[1,2],[1,2]]
- Output: 2
- Explanation: You need to remove two [1,2] to make the rest of the intervals non-overlapping.

### Example 3:
- Input: intervals = [[1,2],[2,3]]
- Output: 0
- Explanation: You don't need to remove any of the intervals since they're already non-overlapping.

## Constraints
- 1 <= intervals.length <= 10^5
- intervals[i].length == 2
- -5 * 10^4 <= starti < endi <= 5 * 10^4

## Solution
```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        if (intervals.length == 0) {
            return 0;
        }

        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));

        int prev = 0;
        int count = 0;

        for (int i = 1; i < intervals.length; i++) {
            if (intervals[prev][1] > intervals[i][0]) {
                if (intervals[prev][1] > intervals[i][1]) {
                    prev = i;
                }
                count++;
            } else {
                prev = i;
            }
        }
        return count;
    }
}
```

## Approach

- Sort the intervals array by the start time of each interval.
- Use a pointer `prev` to track the last kept interval and a counter `count` for removed intervals.
- Iterate through the sorted intervals starting from index 1.
- If the current interval overlaps with the previous interval (previous end time is greater than current start time), an overlap is detected.
- When there's an overlap, remove the interval with the larger end time by keeping the pointer on the interval with the smaller end time. Increment the count.
- If there's no overlap, update the `prev` pointer to the current interval.
- Return the total count of removed intervals.

## Time Complexity

O(n log n), where n is the number of intervals, due to the sorting operation.

## Space Complexity

O(1), as we only use a constant amount of extra space for variables.