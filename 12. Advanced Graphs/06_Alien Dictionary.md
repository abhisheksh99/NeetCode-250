# 269. Alien Dictionary

## Problem Statement
There is a new alien language that uses the English alphabet. However, the order among the letters is unknown to you.

You are given a list of strings `words` from the alien language's dictionary, where the strings in `words` are sorted lexicographically by the rules of this new language.

Return a string of the unique letters in the new alien language sorted in lexicographically increasing order by the new language's rules. If there is no valid ordering, return an empty string. If there are multiple valid orderings, return any of them.

### Examples

**Example 1:**
```
Input: words = ["wrt","wrf","er","ett","rftt"]
Output: "wertf"
```

**Example 2:**
```
Input: words = ["z","x"]
Output: "zx"
```

**Example 3:**
```
Input: words = ["z","x","z"]
Output: ""
Explanation: The order is invalid, so return "".
```

### Constraints:
- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`
- `words[i]` consists of only lowercase English letters.

## Solution
```java
import java.util.*;

class Solution {
    public Map<Character, List<Character>> reversedList = new HashMap<>();
    public Map<Character, Boolean> seen = new HashMap<>();
    public StringBuilder result = new StringBuilder();

    public String foreignDictionary(String[] words) {
        // Initialize the graph with all unique characters
        for (String word : words) {
            for (char c : word.toCharArray()) {
                reversedList.putIfAbsent(c, new ArrayList<>());
            }
        }

        // Build the graph by comparing adjacent words
        for (int i = 0; i < words.length - 1; i++) {
            String word1 = words[i];
            String word2 = words[i + 1];

            // Check for invalid case where word2 is a prefix of word1
            if (word1.length() > word2.length() && word1.startsWith(word2)) {
                return "";
            }

            // Find the first differing character
            for (int j = 0; j < Math.min(word1.length(), word2.length()); j++) {
                if (word1.charAt(j) != word2.charAt(j)) {
                    // Add edge: word2[j] -> word1[j]
                    reversedList.get(word2.charAt(j)).add(word1.charAt(j));
                    break;
                }
            }
        }

        // Perform topological sort using DFS
        for (Character c : reversedList.keySet()) {
            boolean res = dfs(c);
            if (!res) return ""; // Cycle detected
        }

        // Check if we've included all characters
        if (result.length() < reversedList.size()) {
            return "";
        }

        return result.toString();
    }

    // Returns true if no cycle is detected
    public boolean dfs(Character c) {
        // If we've already visited this node, return its state
        if (seen.containsKey(c)) {
            return seen.get(c);
        }

        // Mark as visiting
        seen.put(c, false);

        // Visit all neighbors
        for (Character next : reversedList.get(c)) {
            boolean res = dfs(next);
            if (!res) return false; // Cycle detected in a neighbor
        }

        // Mark as visited and add to result
        seen.put(c, true);
        result.append(c);
        return true;
    }
}
```

## Approach
1. **Graph Construction**:
   - We first initialize a graph where each node represents a unique character in the alien language.
   - We then compare adjacent words to find the first differing character and add an edge from the character in the second word to the character in the first word.

2. **Cycle Detection and Topological Sort**:
   - We perform a DFS-based topological sort to find a valid ordering of characters.
   - We use a `seen` map to track the state of each node (unvisited, visiting, visited).
   - If we encounter a node that is currently being visited, we've found a cycle and return an empty string.

3. **Result Construction**:
   - The result is built in reverse order during the post-order traversal of the graph.
   - We append each character to the result only after all its dependencies have been processed.

## Complexity
- **Time Complexity**: O(C)
  - Where C is the total number of characters across all words.
  - We process each character exactly once during the DFS.
- **Space Complexity**: O(1) or O(U + min(U², N))
  - Where U is the number of unique characters (bounded by 26).
  - The graph stores at most O(U²) edges in the worst case.
  - The recursion stack can go up to U levels deep.

## Edge Cases
- Single word: `["a"]` → `"a"`
- Multiple words with same prefix: `["abc", "ab"]` → `""` (invalid)
- Disconnected graph: `["a", "b", "c"]` → any valid order like `"abc"`
- All words same: `["abc", "abc"]` → `"abc"`

## Key Insights
- The problem reduces to finding a valid topological sort of characters based on their relative ordering in the dictionary.
- The graph is built by comparing adjacent words and finding the first differing character.
- The solution efficiently detects cycles during the topological sort to handle invalid cases.
- The use of a reversed adjacency list and post-order traversal ensures the correct character ordering in the result.