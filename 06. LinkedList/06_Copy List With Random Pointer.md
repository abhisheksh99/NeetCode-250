# 138. Copy List with Random Pointer
**Medium**

## Problem Statement
A linked list of length n is given such that each node contains an additional random pointer, which could point to any node in the list or null.

Construct a deep copy of the list. The deep copy should consist of exactly n brand new nodes, where each new node has its value set to the value of its corresponding original node. Both the next and random pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. None of the pointers in the new list should point to nodes in the original list.

Return the head of the copied linked list.

## Examples
### Example 1:
- Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
- Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]

### Example 2:
- Input: head = [[1,1],[2,1]]
- Output: [[1,1],[2,1]]

### Example 3:
- Input: head = [[3,null],[3,0],[3,null]]
- Output: [[3,null],[3,0],[3,null]]

## Constraints
- 0 <= n <= 1000
- -10^4 <= Node.val <= 10^4
- Node.random is null or points to a node in the list.

## Solution
```java
/*
// Definition for a Node.
class Node {
	int val;
	Node next;
	Node random;

	public Node(int val) {
		this.val = val;
		this.next = null;
		this.random = null;
	}
}
*/

public class Solution {
	public Node copyRandomList(Node head) {
		if (head == null) return null;
        
		HashMap<Node, Node> oldToNew = new HashMap<>();
        
		Node curr = head;
		while (curr != null) {
			oldToNew.put(curr, new Node(curr.val));
			curr = curr.next;
		}
        
		curr = head;
		while (curr != null) {
			oldToNew.get(curr).next = oldToNew.get(curr.next);
			oldToNew.get(curr).random = oldToNew.get(curr.random);
			curr = curr.next;
		}
        
		return oldToNew.get(head);
	}
}
```

Approach

- Use a HashMap to map original nodes to their copies.
- First pass: create a copy of each node and store in the map.
- Second pass: set the next and random pointers for each copied node using the map.
- Return the copied head node.

Time Complexity

O(n), where n is the number of nodes in the list.

Space Complexity

O(n), for the HashMap storing node mappings.
