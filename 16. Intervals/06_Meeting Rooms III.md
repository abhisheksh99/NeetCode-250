# 2402. Meeting Rooms III
**Hard**

## Problem Statement
You are given an integer `n`. There are `n` rooms numbered from `0` to `n - 1`.

You are given a 2D integer array `meetings` where `meetings[i] = [starti, endi]` means that a meeting will be held during the half-closed time interval `[starti, endi)`. All the values of `starti` are unique.

Meetings are allocated to rooms in the following manner:

1. Each meeting will take place in the unused room with the lowest number.
2. If there are no available rooms, the meeting will be delayed until a room becomes free. The delayed meeting should have the same duration as the original meeting.
3. When a room becomes unused, meetings that have an earlier original start time should be given the room.

Return the number of the room that held the most meetings. If there are multiple rooms, return the room with the lowest number.

## Examples
### Example 1:
- Input: n = 2, meetings = [[0,10],[1,5],[2,7],[3,4]]
- Output: 0
- Explanation: Room 0 holds 3 meetings (meetings 0, 1, and 2), while room 1 holds 1 meeting (meeting 3). Room 0 held the most meetings.

### Example 2:
- Input: n = 3, meetings = [[1,20],[2,10],[3,5],[4,9],[6,8]]
- Output: 1
- Explanation: Room 0 holds 2 meetings, room 1 holds 2 meetings, and room 2 holds 1 meeting. Rooms 0 and 1 both held the most meetings, so we return room 1 since it has the lowest number.

## Constraints
- 1 <= n <= 100
- 1 <= meetings.length <= 10^5
- meetings[i].length == 2
- 0 <= starti < endi <= 5 * 10^5
- All the values of starti are unique.

## Solution
```java
class Solution {
    public int mostBooked(int n, int[][] meetings) {
        // Sort meetings by start time
        Arrays.sort(meetings, (a, b) -> a[0] - b[0]);
        
        // Min-heap to store ongoing meetings [endTime, roomNumber]
        PriorityQueue<long[]> ongoing = new PriorityQueue<>((a,b) -> a[0] == b[0] ? (int)(a[1]-b[1]) : (int)(a[0]-b[0]));
        
        // Min-heap to store free rooms
        PriorityQueue<Integer> freeRooms = new PriorityQueue<>();
        for (int i = 0; i < n; i++) freeRooms.offer(i);
        
        int[] count = new int[n];
        
        for (int[] meeting : meetings) {
            int start = meeting[0];
            int end = meeting[1];
            
            // Free up rooms that have finished before current meeting starts
            while (!ongoing.isEmpty() && ongoing.peek()[0] <= start) {
                freeRooms.offer((int)ongoing.poll()[1]);
            }
            
            // Assign room
            int room = freeRooms.isEmpty() ? (int)ongoing.poll()[1] : freeRooms.poll();
            long actualStart = freeRooms.isEmpty() ? ongoing.peek()[0] : start;
            long actualEnd = Math.max(actualStart, start) + (end - start);
            
            ongoing.offer(new long[]{actualEnd, room});
            count[room]++;
        }
        
        // Find room with max bookings
        int maxBooked = 0;
        int ans = 0;
        for (int i = 0; i < n; i++) {
            if (count[i] > maxBooked) {
                maxBooked = count[i];
                ans = i;
            }
        }
        
        return ans;
    }
}
```

## Approach

- Sort all meetings by their start times to process them in chronological order.
- Use two priority queues: one for tracking ongoing meetings with their end times and room numbers, and another for tracking available free rooms.
- For each meeting, first free up all rooms whose meetings have ended before the current meeting starts.
- Assign the meeting to the lowest numbered free room if available; otherwise, assign it to the room that becomes free earliest.
- If a meeting is delayed, calculate its new end time based on when the room becomes available plus the meeting duration.
- Track the number of meetings held in each room using a count array.
- After processing all meetings, find and return the room with the maximum number of meetings (lowest room number in case of a tie).

## Time Complexity

O(m log m + m log n), where m is the number of meetings and n is the number of rooms. Sorting takes O(m log m), and each meeting involves heap operations that take O(log n).

## Space Complexity

O(n), for storing the priority queues and the count array.