# Intervals - Complete Guide & Patterns

## What are Intervals?
Intervals are pairs of numbers (start, end) representing a range on a number line. They are commonly used to represent periods of time, ranges, or segments in problems.

**Examples:**
- Meeting times: [start, end]
- Ranges: [low, high]
- Bookings, reservations, or events

---

## Common Interval Problem Types
- Merge overlapping intervals
- Insert a new interval
- Find intersections between two interval lists
- Remove covered intervals
- Find minimum number of meeting rooms
- Non-overlapping intervals (scheduling)

---

## Generic Pattern to Solve Interval Problems
1. **Sort the intervals**
   - Most interval problems require sorting by start (and sometimes end) time.
   - Use `Arrays.sort(intervals, (a, b) -> a[0] - b[0]);` in Java.

2. **Iterate and Compare**
   - Use a loop to compare the current interval with the previous (or last in result list).
   - Check for overlap: `if (curr[0] <= prev[1])` (overlap exists).

3. **Merge or Take Action**
   - If overlapping, merge: `prev[1] = Math.max(prev[1], curr[1]);`
   - If not, add the current interval to the result.

4. **Edge Cases**
   - Empty input, single interval, fully overlapping intervals, etc.

---

## Generic Java Template for Merging Intervals
```java
public int[][] merge(int[][] intervals) {
	if (intervals.length == 0) return new int[0][2];
	Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
	List<int[]> res = new ArrayList<>();
	int[] curr = intervals[0];
	for (int i = 1; i < intervals.length; i++) {
		if (intervals[i][0] <= curr[1]) {
			curr[1] = Math.max(curr[1], intervals[i][1]);
		} else {
			res.add(curr);
			curr = intervals[i];
		}
	}
	res.add(curr);
	return res.toArray(new int[res.size()][]);
}
```

---

## Time and Space Complexity
- **Sorting:** O(n log n), where n is the number of intervals.
- **Merging/Processing:** O(n), as each interval is processed once.
- **Total:** O(n log n) time, O(n) space for the result list.

---

## Tips for Interval Problems
- Always sort intervals first unless the problem guarantees sorted input.
- Draw the intervals on a number line to visualize overlaps.
- For meeting rooms or scheduling, use a min-heap to track end times.
- For interval intersections, use two pointers to traverse both lists.
- Practice: Merge Intervals, Insert Interval, Meeting Rooms, Non-overlapping Intervals, Interval List Intersections.
