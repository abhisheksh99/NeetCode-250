# 208. Implement Trie (Prefix Tree)
**Medium**

## Problem Statement
A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:
- Trie() Initializes the trie object.
- void insert(String word) Inserts the string word into the trie.
- boolean search(String word) Returns true if the string word is in the trie (i.e., was inserted before), and false otherwise.
- boolean startsWith(String prefix) Returns true if there is a previously inserted string word that has the prefix prefix, and false otherwise.

## Examples
### Example 1:
- Input: ["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
  [["Trie"], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
- Output: [null, null, true, false, true, null, true]

## Constraints
- 1 <= word.length, prefix.length <= 2000
- word and prefix consist only of lowercase English letters.
- At most 3 * 10^4 calls will be made to insert, search, and startsWith.

## Solution
```java
public class TrieNode {
    HashMap<Character, TrieNode> children = new HashMap<>();
    boolean endOfWord = false;
}
public class PrefixTree {
    private TrieNode root;
    public PrefixTree() {
        root = new TrieNode();
    }
    public void insert(String word) {
        TrieNode cur = root;
        for (char c : word.toCharArray()) {
            cur.children.putIfAbsent(c, new TrieNode());
            cur = cur.children.get(c);
        }
        cur.endOfWord = true;
    }
    public boolean search(String word) {
        TrieNode cur = root;
        for (char c : word.toCharArray()) {
            if (!cur.children.containsKey(c)) {
                return false;
            }
            cur = cur.children.get(c);
        }
        return cur.endOfWord;
    }
    public boolean startsWith(String prefix) {
        TrieNode cur = root;
        for (char c : prefix.toCharArray()) {
            if (!cur.children.containsKey(c)) {
                return false;
            }
            cur = cur.children.get(c);
        }
        return true;
    }
}
```

Approach
- Use a TrieNode class with a HashMap for children and a boolean for end-of-word.
- For insert, traverse or create nodes for each character.
- For search, traverse nodes and check end-of-word.
- For startsWith, traverse nodes for the prefix.

Time Complexity
- Insert, Search, startsWith: O(L), where L is the length of the word or prefix.

Space Complexity
- O(N), where N is the total number of characters inserted.
