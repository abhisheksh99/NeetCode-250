# 212. Word Search II
**Hard**

## Problem Statement
Given an m x n board of characters and a list of strings words, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

## Examples
### Example 1:
- Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
- Output: ["eat","oath"]

### Example 2:
- Input: board = [["a","b"],["c","d"]], words = ["abcb"]
- Output: []

## Constraints
- m == board.length
- n == board[i].length
- 1 <= m, n <= 12
- 1 <= words.length <= 3 * 10^4
- 1 <= words[i].length <= 10
- board and words[i] consist of lowercase English letters.

## Solution
```java
class TrieNode{
    HashMap<Character, TrieNode> children = new HashMap<Character, TrieNode>();
    String word = null;
    public TrieNode(){}
}
class Solution {
    char[][] fullboard = null;
    ArrayList<String> answer = new ArrayList<String>();
    public List<String> findWords(char[][] board, String[] words) {
        TrieNode root = new TrieNode();
        for(String word: words){
            TrieNode node = root;
            for(Character letter: word.toCharArray()){
                if(node.children.containsKey(letter)){
                    node = node.children.get(letter);
                }
                else{
                    TrieNode newNode = new TrieNode();
                    node.children.put(letter, newNode);
                    node = newNode;
                }
            }
            node.word = word;
        }
        this.fullboard = board;
        for(int row =0; row<board.length; row++){
            for(int col=0; col<board[0].length; col++){
               if(root.children.containsKey(board[row][col])){
                   backtracking(row,col,root);
               } 
            }
        }
        return answer;
    }
    public void backtracking(int row, int col, TrieNode parent){
        Character letter = fullboard[row][col];
        TrieNode currNode = parent.children.get(letter);
        if(currNode.word != null){
            answer.add(currNode.word);
            currNode.word=null;
        }
        fullboard[row][col] = '#';
        int[] rowOffset = {-1,0,1,0};
        int[] colOffset = {0,1,0,-1};
        for(int i=0; i<4; i++){
            int newRow = row+rowOffset[i];
            int newCol = col+colOffset[i];
            if(newRow <0 || newCol <0 || newRow >= fullboard.length || newCol >= fullboard[0].length){
                continue;
            }
            if(currNode.children.containsKey(fullboard[newRow][newCol])){
                backtracking(newRow, newCol, currNode);
            }
        }
        fullboard[row][col] = letter;
        if(currNode.children.isEmpty()){
            parent.children.remove(letter);
        }
    }
}
```

Approach
- Build a Trie from the list of words.
- For each cell in the board, start backtracking if the cell's letter is a Trie child.
- Use DFS to explore all possible paths, marking visited cells.
- Add found words to the answer and prune Trie branches to optimize.

Time Complexity
- O(m * n * 4^L), where m and n are board dimensions and L is the max word length (pruned by Trie).

Space Complexity
- O(N + m * n), where N is the total number of Trie nodes.
