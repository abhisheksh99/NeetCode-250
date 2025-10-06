# 1851. Minimum Interval to Include Each Query
**Hard**

## Problem Statement
You are given a 2D integer array `intervals`, where `intervals[i] = [lefti, righti]` describes the ith interval starting at `lefti` and ending at `righti` (inclusive). The size of an interval is defined as the number of integers it contains, or more formally `righti - lefti + 1`.

You are also given an integer array `queries`. The answer to the jth query is the size of the smallest interval `i` such that `lefti <= queries[j] <= righti`. If no such interval exists, the answer is -1.

Return an array containing the answers to the queries.

## Examples
### Example 1:
- Input: intervals = [[1,4],[2,4],[3,6],[4,4]], queries = [2,3,4,5]
- Output: [3,3,1,4]
- Explanation: The queries are processed as follows:
  - Query = 2: The interval [2,4] is the smallest interval containing 2. The answer is 4 - 2 + 1 = 3.
  - Query = 3: The interval [2,4] is the smallest interval containing 3. The answer is 4 - 2 + 1 = 3.
  - Query = 4: The interval [4,4] is the smallest interval containing 4. The answer is 4 - 4 + 1 = 1.
  - Query = 5: The interval [3,6] is the smallest interval containing 5. The answer is 6 - 3 + 1 = 4.

### Example 2:
- Input: intervals = [[2,3],[2,5],[1,8],[20,25]], queries = [2,19,5,22]
- Output: [2,-1,4,6]
- Explanation: The queries are processed as follows:
  - Query = 2: The interval [2,3] is the smallest interval containing 2. The answer is 3 - 2 + 1 = 2.
  - Query = 19: None of the intervals contain 19. The answer is -1.
  - Query = 5: The interval [2,5] is the smallest interval containing 5. The answer is 5 - 2 + 1 = 4.
  - Query = 22: The interval [20,25] is the smallest interval containing 22. The answer is 25 - 20 + 1 = 6.

## Constraints
- 1 <= intervals.length <= 10^5
- 1 <= queries.length <= 10^5
- intervals[i].length == 2
- 1 <= lefti <= righti <= 10^7
- 1 <= queries[j] <= 10^7

## Solution
```java
public class Solution {
    public int[] minInterval(int[][] intervals, int[] queries) {
        Arrays.sort(intervals, Comparator.comparingInt(a -> a[0]));
        PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> Integer.compare(a[0], b[0]));
        Map<Integer, Integer> res = new HashMap<>();
        int i = 0;
        for (int q : Arrays.stream(queries).sorted().toArray()) {
            while (i < intervals.length && intervals[i][0] <= q) {
                int l = intervals[i][0];
                int r = intervals[i][1];
                minHeap.offer(new int[]{r - l + 1, r});
                i++;
            }
            while (!minHeap.isEmpty() && minHeap.peek()[1] < q) {
                minHeap.poll();
            }
            res.put(q, minHeap.isEmpty() ? -1 : minHeap.peek()[0]);
        }
        int[] result = new int[queries.length];
        for (int j = 0; j < queries.length; j++) {
            result[j] = res.get(queries[j]);
        }
        return result;
    }
}
```

## Approach

- Sort the intervals by their start positions to process them in order.
- Create a min-heap to store intervals by their size, with each entry containing the interval size and end position.
- Sort the queries and process them in ascending order, using a hash map to store results.
- For each query, add all intervals that start at or before the query position to the min-heap.
- Remove intervals from the heap that end before the current query position, as they don't contain the query.
- The top of the min-heap contains the smallest interval that includes the current query. Store this size in the result map.
- After processing all queries, construct the result array by looking up each original query in the result map.

## Time Complexity

O(n log n + m log m + (n + m) log n), where n is the number of intervals and m is the number of queries. Sorting intervals and queries takes O(n log n) and O(m log m), and heap operations take O(log n) per query.

## Space Complexity

O(n + m), for storing the min-heap, result map, and the sorted queries array.