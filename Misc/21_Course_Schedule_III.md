# 630. Course Schedule III

## Problem Statement
There are `n` different online courses numbered from `1` to `n`. You are given an array `courses` where `courses[i] = [durationi, lastDayi]` indicate that the `i-th` course should be taken continuously for `durationi` days and must be finished before or on `lastDayi`.

You will start on day `1` and cannot take two or more courses simultaneously.

Return the maximum number of courses that you can take.

### Examples

#### Example 1:
```
Input: courses = [[100,200],[200,1300],[1000,1250],[2000,3200]]
Output: 3
Explanation: 
There are totally 4 courses, but you can take 3 courses at most:
- First, take the 1st course in 100 days, finish it on day 100.
- Second, take the 3rd course in 1000 days, finish it on day 1100.
- Third, take the 2nd course in 200 days, finish it on day 1300.
The 4th course cannot be taken now, since you will finish it on day 3300, which exceeds the last day 3200.
```

#### Example 2:
```
Input: courses = [[1,2]]
Output: 1
```

#### Example 3:
```
Input: courses = [[3,2],[4,3]]
Output: 0
```

## Approach
This problem can be solved using a greedy approach with a max-heap. The key insight is to prioritize courses with earlier deadlines and use a max-heap to replace longer duration courses when necessary.

1. **Sort the courses by their deadlines**: This allows us to process courses in the order of their deadlines, ensuring we always have the optimal selection of courses up to the current deadline.

2. **Use a max-heap to track the durations of selected courses**: As we process each course, we add its duration to a running total. If adding a course causes the total duration to exceed its deadline, we remove the longest course from our selection (using the max-heap) to make room for the current course if it's shorter.

3. **Track the maximum number of courses**: The size of the heap at the end of the process gives us the maximum number of courses that can be taken.

### Key Insights
- **Greedy Selection**: By processing courses in order of their deadlines, we ensure that we always have the most flexibility in scheduling.
- **Max-Heap for Optimization**: The max-heap helps us efficiently manage the durations of selected courses, allowing us to replace longer courses with shorter ones when necessary to fit more courses.
- **Time Complexity**: The solution efficiently handles the constraints with a time complexity of O(n log n) due to sorting and heap operations.

## Solution Code
```java
import java.util.Arrays;
import java.util.PriorityQueue;

class Solution {
    public int scheduleCourse(int[][] courses) {
        // Sort courses by their deadlines
        Arrays.sort(courses, (a, b) -> a[1] - b[1]);
        
        // Max-heap to store the durations of selected courses
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
        
        int time = 0;
        for (int[] course : courses) {
            int duration = course[0];
            int lastDay = course[1];
            
            // If we can take the current course, add it to the heap
            if (time + duration <= lastDay) {
                time += duration;
                maxHeap.offer(duration);
            } 
            // If we can't take the current course, check if we can replace a longer course
            else if (!maxHeap.isEmpty() && maxHeap.peek() > duration) {
                // Remove the longest course and add the current one
                time += duration - maxHeap.poll();
                maxHeap.offer(duration);
            }
        }
        
        return maxHeap.size();
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(n log n), where n is the number of courses. The sorting step takes O(n log n) time, and each heap operation takes O(log n) time, with up to n operations.
- **Space Complexity**: O(n), for the max-heap which can store up to n elements in the worst case.

## Key Insights
1. **Greedy Selection**: The algorithm makes the optimal choice at each step by processing courses in order of their deadlines, ensuring we always have the most flexibility in scheduling.
2. **Max-Heap for Optimization**: The max-heap efficiently manages the durations of selected courses, allowing us to replace longer courses with shorter ones when necessary to fit more courses.
3. **Edge Cases**: The solution handles edge cases such as no courses, a single course, and cases where no courses can be taken due to tight deadlines.

## Edge Cases
1. **No Courses**: If there are no courses, the result should be 0.
2. **Single Course**: If there is only one course and it can be completed before its deadline, the result should be 1.
3. **Tight Deadlines**: If all courses have deadlines that are too tight to complete any course, the result should be 0.
4. **Large Input**: The solution should efficiently handle the maximum constraints (up to 10,000 courses).

## Follow-up
How would you modify the solution if you could take multiple courses simultaneously, but each course must be completed before its deadline? (This would require a different approach, possibly involving interval scheduling or dynamic programming.)