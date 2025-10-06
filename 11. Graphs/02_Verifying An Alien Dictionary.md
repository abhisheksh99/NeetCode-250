# 953. Verifying an Alien Dictionary

## Problem Statement
In an alien language, surprisingly, they also use English lowercase letters, but possibly in a different order. The order of the alphabet is some permutation of lowercase letters.

Given a sequence of words written in the alien language, and the order of the alphabet, return `true` if and only if the given words are sorted lexicographically in this alien language.

### Examples

**Example 1:**
```
Input: words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"
Output: true
Explanation: As 'h' comes before 'l' in this language, then the sequence is sorted.
```

**Example 2:**
```
Input: words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
Output: false
Explanation: As 'd' comes after 'l' in this language, then words[0] > words[1], hence the sequence is unsorted.
```

**Example 3:**
```
Input: words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"
Output: false
Explanation: The first three characters "app" match, and the second string is shorter (in size).
```

### Constraints:
- `1 <= words.length <= 100`
- `1 <= words[i].length <= 20`
- `order.length == 26`
- All characters in `words[i]` and `order` are English lowercase letters.

## Solution
```java
import java.util.*;

class Solution {
    public boolean isAlienSorted(String[] words, String order) {
        Map<Character, Integer> orderMap = new HashMap<>();
        
        // Create a mapping from character to its position in the order string
        for (int i = 0; i < order.length(); i++) {
            orderMap.put(order.charAt(i), i);
        }
        
        // Compare each adjacent pair of words
        for (int i = 0; i < words.length - 1; i++) {
            for (int j = 0; j < words[i].length(); j++) {
                // If we've reached the end of the next word, it's invalid
                if (j >= words[i + 1].length()) {
                    return false;
                }
                
                // Compare characters at the current position
                if (words[i].charAt(j) != words[i + 1].charAt(j)) {
                    int currLetter = orderMap.get(words[i].charAt(j));
                    int nextLetter = orderMap.get(words[i + 1].charAt(j));
                    
                    // If out of order, return false
                    if (nextLetter < currLetter) {
                        return false;
                    } else {
                        // If in order, no need to check remaining characters
                        break;
                    }
                }
            }
        }
        
        return true;
    }
}
```

## Approach
1. **Create Order Mapping**: First, create a hash map that maps each character to its position in the given order string for O(1) lookups.
2. **Compare Adjacent Words**: For each pair of adjacent words in the array:
   - Compare them character by character.
   - If we find a character that differs, check their order using the map.
   - If the order is incorrect, return false immediately.
   - If we reach the end of the first word and it's longer than the second word, return false.
3. **Return Result**: If all adjacent pairs are in order, return true.

## Complexity
- **Time Complexity**: O(M * N), where N is the number of words and M is the maximum length of a word. We compare each character in the worst case.
- **Space Complexity**: O(1), as we only use a fixed-size map (26 letters in the alphabet).

## Edge Cases
- Single word array should return true.
- Empty array should return true.
- Words with same prefix but different lengths (e.g., ["apple", "app"]).
- All words are the same.
- Maximum length words (20 characters).
- Maximum number of words (100).

## Key Insights
- The problem reduces to checking if each adjacent pair of words is in order.
- By creating a mapping of characters to their positions, we can compare characters in O(1) time.
- We only need to compare up to the first differing character between words.
- The order of the first differing character determines the order of the words.