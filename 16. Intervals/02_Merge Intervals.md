# 56. Merge Intervals
**Medium**

## Problem Statement
Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

## Examples
### Example 1:
- Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
- Output: [[1,6],[8,10],[15,18]]
- Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].

### Example 2:
- Input: intervals = [[1,4],[4,5]]
- Output: [[1,5]]
- Explanation: Intervals [1,4] and [4,5] are considered overlapping.

## Constraints
- 1 <= intervals.length <= 10^4
- intervals[i].length == 2
- 0 <= starti <= endi <= 10^4

## Solution
```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals,(a,b) -> Integer.compare(a[0],b[0]));
        List<int[]> ans = new ArrayList<>();

        for(int[] interval:intervals){
            if(ans.isEmpty() || ans.get(ans.size()-1)[1]<interval[0]){
                ans.add(interval);
            }else{
                ans.get(ans.size()-1)[1]= Math.max(ans.get(ans.size()-1)[1],interval[1]);
            }
        }
     return ans.toArray(new int[ans.size()][]);    
    }
}
```

## Approach

- Sort the intervals array by the start time of each interval using a custom comparator.
- Initialize an empty list to store the merged intervals.
- Iterate through each interval in the sorted array.
- If the result list is empty or the last interval in the result does not overlap with the current interval, add the current interval to the result.
- If there is an overlap, merge the intervals by updating the end time of the last interval in the result to be the maximum of both end times.
- Convert the result list to a 2D array and return.

## Time Complexity

O(n log n), where n is the number of intervals, due to the sorting operation.

## Space Complexity

O(n), for storing the merged intervals in the result list.