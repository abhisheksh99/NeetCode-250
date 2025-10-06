# 763. Partition Labels

## Problem Statement
You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part.

Note that the partition is done so that after concatenating all the parts in order, the resultant string should be `s`.

Return a list of integers representing the size of these parts.

### Examples

**Example 1:**
```
Input: s = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.
```

**Example 2:**
```
Input: s = "eccbbbbdec"
Output: [10]
```

### Constraints:
- `1 <= s.length <= 500`
- `s` consists of lowercase English letters.

## Solution
```java
import java.util.*;

public class Solution {
    public List<Integer> partitionLabels(String s) {
        Map<Character, Integer> lastIndex = new HashMap<>();
        for (int i = 0; i < s.length(); i++) {
            lastIndex.put(s.charAt(i), i);
        }

        List<Integer> res = new ArrayList<>();
        int size = 0, end = 0;
        for (int i = 0; i < s.length(); i++) {
            size++;
            end = Math.max(end, lastIndex.get(s.charAt(i)));

            if (i == end) {
                res.add(size);
                size = 0;
            }
        }
        return res;
    }
}
```

## Approach
1. **Store Last Indices**: First, we create a map to store the last occurrence index of each character in the string.
2. **Track Partitions**: We then iterate through the string while keeping track of the current partition's end index. For each character, we update the end of the current partition to be the maximum of the current end and the last occurrence of the current character.
3. **Add Partition Size**: When we reach the end of the current partition (i.e., the current index equals the end of the partition), we add the size of this partition to our result list and reset the size counter for the next partition.

## Complexity
- **Time Complexity**: O(n), where n is the length of the string. We make two passes through the string: one to build the last index map and another to determine the partitions.
- **Space Complexity**: O(1) or O(k), where k is the number of unique characters in the string. In the worst case, if all characters are unique, it would be O(26) = O(1) since there are only 26 lowercase English letters.

## Edge Cases
- A string with all unique characters will result in partitions of size 1 for each character.
- A string with all identical characters will result in a single partition containing the entire string.
- The minimum length string (1 character) will return [1].
- The maximum length string (500 characters) should be handled efficiently within the constraints.
- Strings where characters appear in a way that creates nested partitions (e.g., "abac") should be handled correctly.