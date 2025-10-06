# 332. Reconstruct Itinerary

## Problem Statement
You are given a list of airline `tickets` where `tickets[i] = [from_i, to_i]` represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from `"JFK"`, thus, the itinerary must begin with `"JFK"`. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

### Examples

**Example 1:**
```
Input: tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
Output: ["JFK","MUC","LHR","SFO","SJC"]
```

**Example 2:**
```
Input: tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"] but it is larger in lexical order.
```

### Constraints:
- `1 <= tickets.length <= 300`
- `tickets[i].length == 2`
- `from_i.length == 3`
- `to_i.length == 3`
- `from_i` and `to_i` consist of uppercase English letters.
- `from_i != to_i`

## Solution
```java
import java.util.*;

class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        // Build the graph
        Map<String, LinkedList<String>> graph = new HashMap<>();
        for (List<String> ticket : tickets) {
            graph.computeIfAbsent(ticket.get(0), k -> new LinkedList<>()).add(ticket.get(1));
        }

        // Sort each adjacency list in reverse order to pop lex order smallest
        for (LinkedList<String> dests : graph.values()) {
            Collections.sort(dests, Collections.reverseOrder());
        }

        LinkedList<String> itinerary = new LinkedList<>();
        dfs("JFK", graph, itinerary);
        return itinerary;
    }

    private void dfs(String airport, Map<String, LinkedList<String>> graph, LinkedList<String> itinerary) {
        LinkedList<String> dests = graph.get(airport);
        while (dests != null && !dests.isEmpty()) {
            String nextDest = dests.removeLast();  // Get lex smallest destination due to reverse sort
            dfs(nextDest, graph, itinerary);
        }
        itinerary.addFirst(airport);  // Post-order addition
    }
}
```

## Approach
1. **Graph Construction**:
   - We build a directed graph where each node represents an airport and edges represent flights.
   - The adjacency list is implemented using a `HashMap` where the key is the departure airport and the value is a list of arrival airports.

2. **Sorting for Lexicographical Order**:
   - We sort each list of destinations in reverse order so that we can efficiently retrieve the lexicographically smallest destination using `removeLast()`.

3. **Depth-First Search (DFS)**:
   - We perform a post-order DFS starting from "JFK".
   - For each airport, we recursively visit all its destinations, removing them from the graph as we go.
   - We add the current airport to the itinerary only after visiting all its destinations (post-order).

4. **Result Construction**:
   - The itinerary is built in reverse order during the DFS and then returned as the final result.

## Complexity
- **Time Complexity**: O(E log E)
  - Building the graph takes O(E) time.
  - Sorting each adjacency list takes a total of O(E log E) time in the worst case.
  - The DFS visits each edge exactly once, taking O(E) time.
- **Space Complexity**: O(V + E)
  - The graph requires O(V + E) space.
  - The recursion stack can go up to O(V) in the worst case.
  - The output list takes O(E + 1) space.

## Edge Cases
- Single flight: `[["JFK", "SFO"]]` → `["JFK", "SFO"]`
- Multiple flights from the same origin: `[["JFK", "SFO"], ["JFK", "ATL"]]` → `["JFK", "ATL", "SFO"]`
- Circular itinerary: `[["JFK", "SFO"], ["SFO", "JFK"]]` → `["JFK", "SFO", "JFK"]`
- Multiple valid itineraries: The solution should choose the lexicographically smaller one.

## Key Insights
- The problem can be modeled as finding an Eulerian trail in a directed graph.
- The post-order DFS ensures that we add an airport to the itinerary only after visiting all its outgoing flights.
- Sorting the adjacency lists in reverse order allows efficient retrieval of the lexicographically smallest destination using `removeLast()`.
- The solution efficiently handles the requirement to return the lexicographically smallest itinerary by processing destinations in reverse sorted order.