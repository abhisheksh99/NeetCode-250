# 253. Meeting Rooms II
**Medium**

## Problem Statement
Given an array of meeting time intervals `intervals` where `intervals[i] = [starti, endi]`, return the minimum number of conference rooms required.

## Examples
### Example 1:
- Input: intervals = [[0,30],[5,10],[15,20]]
- Output: 2
- Explanation: One room can hold meetings [0,30] and another room can hold meetings [5,10] and [15,20].

### Example 2:
- Input: intervals = [[7,10],[2,4]]
- Output: 1
- Explanation: Only one room is needed as the meetings do not overlap.

## Constraints
- 1 <= intervals.length <= 10^4
- 0 <= starti < endi <= 10^6

## Solution
```java
/**
 * Definition of Interval:
 * public class Interval {
 *     public int start, end;
 *     public Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 * }
 */

public class Solution {
    public int minMeetingRooms(List<Interval> intervals) {
        int n = intervals.size();
        int[] start = new int[n];
        int[] end = new int[n];

        for (int i = 0; i < n; i++) {
            start[i] = intervals.get(i).start;
            end[i] = intervals.get(i).end;
        }

        Arrays.sort(start);
        Arrays.sort(end);

        int res = 0, count = 0, s = 0, e = 0;
        while (s < n) {
            if (start[s] < end[e]) {
                s++;
                count++;
            } else {
                e++;
                count--;
            }
            res = Math.max(res, count);
        }
        return res;
    }
}
```

## Approach

- Separate the start times and end times of all meetings into two arrays.
- Sort both the start times and end times arrays independently.
- Use two pointers to traverse the start and end arrays simultaneously.
- When a meeting starts before another ends, increment the room count and move the start pointer.
- When a meeting ends before or at the same time as another starts, decrement the room count and move the end pointer.
- Track the maximum number of rooms needed at any point in time.
- Return the maximum room count as the result.

## Time Complexity

O(n log n), where n is the number of intervals, due to the sorting operations on start and end arrays.

## Space Complexity

O(n), for storing the start and end times in separate arrays.