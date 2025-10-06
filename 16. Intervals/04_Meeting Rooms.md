# 252. Meeting Rooms
**Easy**

## Problem Statement
Given an array of meeting time intervals where `intervals[i] = [starti, endi]`, determine if a person could attend all meetings.

## Examples
### Example 1:
- Input: intervals = [[0,30],[5,10],[15,20]]
- Output: false
- Explanation: The person cannot attend all meetings because [0,30] overlaps with [5,10] and [15,20].

### Example 2:
- Input: intervals = [[7,10],[2,4]]
- Output: true
- Explanation: The meetings do not overlap, so the person can attend all of them.

## Constraints
- 0 <= intervals.length <= 10^4
- intervals[i].length == 2
- 0 <= starti < endi <= 10^6

## Solution
```java
class Solution {
    public boolean canAttendMeetings(List<Interval> intervals) {
        intervals.sort((a,b) -> Integer.compare(a.start, b.start));
        
        for (int i = 0; i < intervals.size() - 1; i++) {
            if (intervals.get(i).end > intervals.get(i+1).start) {
                return false;
            }
        }
        return true;
    }
}
```

## Approach

- Sort the intervals by their start times in ascending order.
- Iterate through the sorted intervals and check if any meeting overlaps with the next one.
- If the end time of the current meeting is greater than the start time of the next meeting, there is an overlap, so return false.
- If no overlaps are found after checking all consecutive pairs, return true.

## Time Complexity

O(n log n), where n is the number of intervals, due to the sorting operation.

## Space Complexity

O(1), as we only use a constant amount of extra space for the iteration.