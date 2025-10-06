# 49. Group Anagrams
**Medium**

## Problem Statement
Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.
An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

## Examples
### Example 1:
- Input: strs = ["eat","tea","tan","ate","nat","bat"]
- Output: [["eat","tea","ate"],["tan","nat"],["bat"]]

### Example 2:
- Input: strs = [""]
- Output: [[""]]

### Example 3:
- Input: strs = ["a"]
- Output: [["a"]]

## Constraints
- 1 <= strs.length <= 10^4
- 0 <= strs[i].length <= 100
- strs[i] consists of lowercase English letters.

## Solution
```java
class Solution {
	public List<List<String>> groupAnagrams(String[] strs) {
		Map<String,List<String>> map = new HashMap<>();

		for(String word:strs){
			char[] chars=word.toCharArray();
			Arrays.sort(chars);
			String sortedWords=new String(chars);

			if(!map.containsKey(sortedWords)){
				map.put(sortedWords,new ArrayList<>());
			}
			map.get(sortedWords).add(word);
		}
		return new ArrayList<>(map.values());
	}
}
```

Approach

- Initialize a HashMap to group words by their sorted character sequence.
- For each word in the input array:
  - Convert the word to a character array and sort it.
  - Use the sorted string as a key in the map.
  - If the key does not exist, create a new list for it.
  - Add the original word to the list corresponding to the sorted key.
- After processing all words, return the values of the map as the grouped anagrams.
- Notes: Sorting each word ensures all anagrams share the same key.

Time Complexity

O(NK log K), where N is the number of strings and K is the maximum string length (due to sorting each string).

Space Complexity

O(NK), for storing all strings in the map and result.
