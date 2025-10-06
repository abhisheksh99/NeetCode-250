# 211. Design Add and Search Words Data Structure
**Medium**

## Problem Statement
Design a data structure that supports adding new words and finding if a string matches any previously added string. The search string may contain the dot character '.' which can represent any one letter.

Implement the WordDictionary class:
- WordDictionary() Initializes the object.
- void addWord(String word) Adds word to the data structure.
- boolean search(String word) Returns true if there is any string in the data structure that matches word or false otherwise. A dot '.' can match any letter.

## Examples
### Example 1:
- Input: ["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
  [[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
- Output: [null,null,null,null,false,true,true,true]

## Constraints
- 1 <= word.length <= 25
- word in addWord consists of lowercase English letters.
- word in search consists of '.' or lowercase English letters.
- At most 10^4 calls will be made to addWord and search.

## Solution
```java
class TrieNode {
    Map<Character, TrieNode> children = new HashMap<>();
    boolean word = false;
    public TrieNode() {}
}
class WordDictionary {
    TrieNode trie;
    public WordDictionary() {
        trie = new TrieNode();
    }
    public void addWord(String word) {
        TrieNode node = trie;
        for (char ch : word.toCharArray()) {
            if (!node.children.containsKey(ch)) {
                node.children.put(ch, new TrieNode());
            }
            node = node.children.get(ch);
        }
        node.word = true;
    }
    public boolean searchInNode(String word, TrieNode node) {
        for (int i = 0; i < word.length(); i++) {
            char ch = word.charAt(i);
            if (!node.children.containsKey(ch)) {
                if (ch == '.') {
                    for (char x : node.children.keySet()) {
                        TrieNode child = node.children.get(x);
                        if (searchInNode(word.substring(i + 1), child)) {
                            return true;
                        }
                    }
                }
                return false;
            } else {
                node = node.children.get(ch);
            }
        }
        return node.word;
    }
    public boolean search(String word) {
        return searchInNode(word, trie);
    }
}
```

Approach
- Use a TrieNode class with a HashMap for children and a boolean for end-of-word.
- For addWord, insert each character into the trie.
- For search, use recursion to handle the '.' wildcard, trying all possible children when a dot is encountered.
- Return true if a matching word is found.

Time Complexity
- addWord: O(L), where L is the length of the word.
- search: O(26^d * L), where d is the number of '.' in the word and L is the word length.

Space Complexity
- O(N), where N is the total number of characters inserted.
