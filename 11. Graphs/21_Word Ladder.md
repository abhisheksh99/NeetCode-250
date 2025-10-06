# 127. Word Ladder

## Problem Statement
A transformation sequence from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

- Every adjacent pair of words differs by a single letter.
- Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return the number of words in the shortest transformation sequence from `beginWord` to `endWord`, or `0` if no such sequence exists.

### Examples

**Example 1:**
```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> "cog", which is 5 words long.
```

**Example 2:**
```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: 0
Explanation: The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.
```

### Constraints:
- `1 <= beginWord.length <= 10`
- `endWord.length == beginWord.length`
- `1 <= wordList.length <= 5000`
- `wordList[i].length == beginWord.length`
- `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
- `beginWord != endWord`
- All the words in `wordList` are unique.

## Solution
```java
import java.util.*;

class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        // Since all words are of same length.
        int L = beginWord.length();

        // Dictionary to hold combination of words that can be formed,
        // from any given word. By changing one letter at a time.
        Map<String, List<String>> allComboDict = new HashMap<>();

        wordList.forEach( word -> {
            for (int i = 0; i < L; i++) {
                // Key is the generic word
                // Value is a list of words which have the same intermediate generic word.
                String newWord = word.substring(0, i) + '*' + word.substring(i + 1, L);
                List<String> transformations = allComboDict.getOrDefault(newWord, new ArrayList<>());
                transformations.add(word);
                allComboDict.put(newWord, transformations);
            }
        });

        // Queue for BFS
        Queue<Pair<String, Integer>> Q = new LinkedList<>();
        Q.add(new Pair(beginWord, 1));

        // Visited to make sure we don't repeat processing same word.
        Map<String, Boolean> visited = new HashMap<>();
        visited.put(beginWord, true);

        while (!Q.isEmpty()) {
            Pair<String, Integer> node = Q.remove();
            String word = node.getKey();
            int level = node.getValue();
            for (int i = 0; i < L; i++) {
                // Intermediate words for current word
                String newWord = word.substring(0, i) + '*' + word.substring(i + 1, L);

                // Next states are all the words which share the same intermediate state.
                for (String adjacentWord : allComboDict.getOrDefault(newWord, new ArrayList<>())) {
                    // If at any point if we find what we are looking for
                    // i.e. the end word - we can return with the answer.
                    if (adjacentWord.equals(endWord)) {
                        return level + 1;
                    }
                    // Otherwise, add it to the BFS Queue. Also mark it visited
                    if (!visited.containsKey(adjacentWord)) {
                        visited.put(adjacentWord, true);
                        Q.add(new Pair(adjacentWord, level + 1));
                    }
                }
            }
        }

        return 0;
    }
}
```

## Approach
1. **Preprocessing**:
   - Create a dictionary where each key is a generic word (with one letter replaced by '*') and the value is a list of words that match that pattern.
   - This allows for efficient lookup of all words that differ by one character.

2. **Breadth-First Search (BFS)**:
   - Start BFS from the `beginWord` with a level of 1.
   - For each word, generate all possible intermediate words by replacing each character with '*'.
   - For each intermediate word, get all words that match this pattern from the dictionary.
   - If we find the `endWord`, return the current level + 1.
   - Otherwise, add unvisited words to the queue with an incremented level.

3. **Termination**:
   - If the queue is exhausted without finding the `endWord`, return 0.

## Complexity
- **Time Complexity**: O(M² × N), where M is the length of each word and N is the number of words.
  - Preprocessing takes O(M² × N) time.
  - BFS takes O(M² × N) time in the worst case.
- **Space Complexity**: O(M² × N) to store the allComboDict.

## Edge Cases
- `beginWord` equals `endWord` (handled by problem constraints).
- `endWord` not in `wordList`.
- No valid transformation sequence exists.
- Large word lists with maximum constraints.
- Words with all characters different from `beginWord`.

## Key Insights
- The problem can be modeled as an unweighted, undirected graph where each node is a word and edges connect words that differ by one character.
- Using BFS guarantees the shortest path in an unweighted graph.
- The preprocessing step with wildcards makes the neighbor lookup efficient.
- The solution handles all edge cases and constraints effectively.