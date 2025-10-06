# 133. Clone Graph

## Problem Statement
Given a reference of a node in a connected undirected graph. Return a deep copy (clone) of the graph.

Each node in the graph contains a value (`int`) and a list (`List[Node]`) of its neighbors.

```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

### Examples

**Example 1:**
```
Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
```

**Example 2:**
```
Input: adjList = [[]]
Output: [[]]
Explanation: Note that the input contains one empty list. The graph consists of only one node with val = 1 and it does not have any neighbors.
```

**Example 3:**
```
Input: adjList = []
Output: []
Explanation: This an empty graph, it does not have any nodes.
```

### Constraints:
- The number of nodes in the graph is in the range `[0, 100]`.
- `1 <= Node.val <= 100`
- `Node.val` is unique for each node.
- There are no repeated edges and no self-loops in the graph.
- The Graph is connected and all nodes can be visited starting from the given node.

## Solution
```java
/*
Definition for a Node.
*/
class Node {
    public int val;
    public List<Node> neighbors;
    
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}

class Solution {
    private HashMap<Node, Node> visited = new HashMap<>();

    public Node cloneGraph(Node node) {
        // Handle empty graph case
        if (node == null) {
            return null;
        }

        // If node is already cloned, return the clone
        if (visited.containsKey(node)) {
            return visited.get(node);
        }

        // Create a clone for the given node
        Node cloneNode = new Node(node.val, new ArrayList<>());
        // Add the original node and its clone to the visited map
        visited.put(node, cloneNode);

        // Recursively clone all neighbors
        for (Node neighbor : node.neighbors) {
            cloneNode.neighbors.add(cloneGraph(neighbor));
        }

        return cloneNode;
    }
}
```

## Approach
1. **Base Case Handling**: If the input node is null, return null.
2. **Memoization Check**: Use a HashMap to keep track of visited nodes to avoid cycles and redundant processing.
3. **Node Cloning**: Create a new node with the same value as the original node.
4. **Neighbor Cloning**: Recursively clone all neighbors of the current node and add them to the clone's neighbors list.
5. **Return Cloned Node**: Return the cloned node which now has all its neighbors properly cloned.

## Complexity
- **Time Complexity**: O(N + M), where N is the number of nodes and M is the number of edges. We visit each node and edge exactly once.
- **Space Complexity**: O(N), where N is the number of nodes. This is used by the recursion stack and the visited hash map.

## Edge Cases
- Empty graph (null input).
- Graph with a single node and no edges.
- Graph with cycles.
- Graph with multiple connected components (though the problem states the graph is connected).
- Graph with the maximum number of nodes (100).

## Key Insights
- The solution uses Depth-First Search (DFS) to traverse the graph.
- A HashMap is used to map original nodes to their clones, which serves two purposes:
  1. Prevents getting stuck in cycles by checking if a node has already been cloned.
  2. Ensures that each node is only cloned once, making the solution efficient.
- The recursive approach naturally handles the graph traversal and cloning process.