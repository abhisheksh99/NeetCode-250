# 752. Open the Lock

## Problem Statement
You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: `'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'`. The wheels can rotate freely and wrap around: for example, turning `'9'` upwards makes it `'0'`, and turning `'0'` downwards makes it `'9'`.

Initially, the lock is set to `"0000"`, a string representing the state of the 4 wheels.

You are given a list of `deadends` dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a `target` representing the value of the wheels that will unlock the lock, return the minimum number of turns required to open the lock, or `-1` if it is impossible.

### Examples

**Example 1:**
```
Input: deadends = ["0201","0101","0102","1212","2002"], target = "0202"
Output: 6
Explanation:
A sequence of valid moves would be "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202".
Note that a sequence like "0000" -> "0001" -> "0002" -> "0102" -> "0202" would be invalid,
because the wheels become stuck after the display becomes the dead end "0102".
```

**Example 2:**
```
Input: deadends = ["8888"], target = "0009"
Output: 1
Explanation: Turn the last wheel one position forward to make "0000" -> "0009".
```

**Example 3:**
```
Input: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
Output: -1
Explanation: It is impossible to reach the target without getting stuck.
```

### Constraints:
- `1 <= deadends.length <= 500`
- `deadends[i].length == 4`
- `target.length == 4`
- The target will not be in the list `deadends`.
- `target` and `deadends[i]` consist of digits only.

## Solution
```java
import java.util.*;

class Solution {
    public int openLock(String[] deadends, String target) {
        // Convert deadends array to a set for O(1) lookups
        Set<String> deadSet = new HashSet<>(Arrays.asList(deadends));
        
        // Check if the initial state is a deadend
        if (deadSet.contains("0000")) return -1;
        
        // If target is already the initial state
        if (target.equals("0000")) return 0;
        
        // Initialize BFS
        Queue<String> queue = new LinkedList<>();
        queue.offer("0000");
        Set<String> visited = new HashSet<>();
        visited.add("0000");
        
        int moves = 0;
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            moves++;
            
            // Process all nodes at the current level
            for (int i = 0; i < size; i++) {
                String curr = queue.poll();
                
                // Generate all possible next states
                for (String next : getNextStates(curr)) {
                    // Skip if already visited or is a deadend
                    if (!visited.contains(next) && !deadSet.contains(next)) {
                        if (next.equals(target)) {
                            return moves;
                        }
                        queue.offer(next);
                        visited.add(next);
                    }
                }
            }
        }
        
        // If we've exhausted all possibilities and haven't found the target
        return -1;
    }
    
    // Helper method to generate all possible next states from current state
    private List<String> getNextStates(String state) {
        List<String> nextStates = new ArrayList<>();
        char[] arr = state.toCharArray();
        
        // For each wheel position
        for (int i = 0; i < 4; i++) {
            char original = arr[i];
            
            // Turn wheel forward (0->1->...->9->0)
            arr[i] = original == '9' ? '0' : (char)(original + 1);
            nextStates.add(new String(arr));
            
            // Turn wheel backward (0->9->...->1->0)
            arr[i] = original == '0' ? '9' : (char)(original - 1);
            nextStates.add(new String(arr));
            
            // Restore original character for next iteration
            arr[i] = original;
        }
        
        return nextStates;
    }
}
```

## Approach
1. **Initialization**:
   - Convert the `deadends` array to a set for O(1) lookups.
   - Check if the initial state ("0000") is a deadend or if it's already the target.

2. **Breadth-First Search (BFS)**:
   - Use a queue to perform BFS starting from "0000".
   - Track visited states to avoid cycles.
   - For each state, generate all possible next states by turning each wheel up or down.
   - If the target is found, return the number of moves.
   - If the queue is exhausted without finding the target, return -1.

3. **Generating Next States**:
   - For each wheel position (0-3), generate two new states:
     - One by turning the wheel forward (incrementing)
     - One by turning the wheel backward (decrementing)
   - Handle wrapping around (9->0 and 0->9).

## Complexity
- **Time Complexity**: O(10^4) = O(1), since there are 10,000 possible combinations (0000 to 9999).
- **Space Complexity**: O(10^4) for the visited set and queue in the worst case.

## Edge Cases
- The initial state ("0000") is a deadend.
- The target is the same as the initial state.
- All possible combinations are deadends.
- Large number of deadends that block most paths.

## Key Insights
- BFS is ideal for finding the shortest path in an unweighted graph.
- The state space is small (10,000 possible states), making BFS feasible.
- Using a set for deadends and visited states ensures O(1) lookups.
- The `getNextStates` method efficiently generates all possible next states by considering each wheel position independently.