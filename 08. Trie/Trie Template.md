# Trie (Prefix Tree) - Complete Guide & Template

## What is a Trie?
A Trie (pronounced "try"), or prefix tree, is a tree-like data structure used to efficiently store and retrieve keys in a dataset of strings. Each node represents a character of a string, and paths from the root to leaves represent words.

## Use Cases
- Autocomplete and spell-check
- Word search and prefix search
- Dictionary word storage
- IP routing (longest prefix match)
- Solving problems involving substrings, prefixes, or word matching

## Trie Node Structure (Java)
```java
class TrieNode {
	Map<Character, TrieNode> children = new HashMap<>();
	boolean isEndOfWord = false;
	// Optionally, store word or count for advanced problems
}
```

## General Trie Template (Insert, Search, StartsWith)
```java
class Trie {
	private TrieNode root;
	public Trie() {
		root = new TrieNode();
	}
	public void insert(String word) {
		TrieNode node = root;
		for (char c : word.toCharArray()) {
			node.children.putIfAbsent(c, new TrieNode());
			node = node.children.get(c);
		}
		node.isEndOfWord = true;
	}
	public boolean search(String word) {
		TrieNode node = root;
		for (char c : word.toCharArray()) {
			if (!node.children.containsKey(c)) return false;
			node = node.children.get(c);
		}
		return node.isEndOfWord;
	}
	public boolean startsWith(String prefix) {
		TrieNode node = root;
		for (char c : prefix.toCharArray()) {
			if (!node.children.containsKey(c)) return false;
			node = node.children.get(c);
		}
		return true;
	}
}
```

## Common Operations
- **Insert:** Add a word character by character, creating nodes as needed.
- **Search:** Traverse the Trie for the word; return true if isEndOfWord is true at the end.
- **StartsWith:** Traverse for the prefix; return true if all prefix characters are found.
- **Delete (optional):** Recursively remove nodes if they are not part of another word.
- **Wildcard Search:** Use DFS/backtracking for '.' or '?' wildcards.
- **Word Count/Prefix Count:** Store counters at nodes for advanced queries.

## Time and Space Complexity
- **Insert/Search/StartsWith:** O(L), where L is the length of the word/prefix.
- **Space:** O(N * L * A), where N is the number of words, L is average word length, and A is the alphabet size (e.g., 26 for lowercase English).
	- Each node can have up to A children.
	- Space can be optimized with arrays for fixed alphabets or HashMaps for sparse alphabets.

## Interview Tips
- Always clarify the alphabet size and whether words are only lowercase/uppercase or include other characters.
- For wildcard or regex search, use recursion/DFS from the current node.
- For word search in a grid, build a Trie from the word list and use backtracking.
- For prefix/suffix problems, consider using two Tries or a Trie and a reversed Trie.
- Be able to write insert, search, and startsWith from scratch.
- Know how to optimize for memory (array vs. HashMap, pruning unused nodes).

## Advanced Patterns
- **Compressed Trie (Radix Tree):** Merge chains of single-child nodes to save space.
- **Aho-Corasick Algorithm:** For multiple pattern matching in a text.
- **Suffix Trie/Tree:** For substring and pattern matching problems.

---

## Example Problem Patterns
- Word search in a 2D board (LeetCode 212)
- Design add and search word with wildcards (LeetCode 211)
- Longest word with all prefixes present
- Replace words in a sentence with dictionary roots

---

## Summary
Tries are powerful for prefix-based and word-matching problems. Master the basic template, understand how to adapt for wildcards and advanced queries, and be ready to optimize for space and performance in interviews.
