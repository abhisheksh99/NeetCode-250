---

# Tree Interview Checklist & Advanced Topics

## Common Interview Patterns & Advanced Topics
- **Lowest Common Ancestor (LCA):** Find the lowest node that is an ancestor of two given nodes.
- **Binary Search Tree (BST) Properties:** Validate BST, find kth smallest/largest, predecessor/successor.
- **Tree Construction:** Build tree from preorder/inorder/postorder/level order traversals.
- **Serialization/Deserialization:** Convert tree to string and back (for storage or transmission).
- **Path Problems:**
	- Path sum (root-to-leaf, any-to-any)
	- Maximum path sum
	- All root-to-leaf paths
- **Subtree Problems:**
	- Check if one tree is a subtree of another
	- Find duplicate subtrees
- **Symmetry & Balance:**
	- Check if tree is symmetric (mirror)
	- Check if tree is balanced
- **Morris Traversal:** Inorder traversal with O(1) space (advanced, rarely asked but good to know).
- **Threaded Binary Trees:** (Advanced, rarely asked)
- **Trie (Prefix Tree):** For string and prefix problems.
- **Segment Tree/Fenwick Tree (Binary Indexed Tree):** For range queries and updates (advanced, for competitive programming).
- **N-ary Trees:** Trees where nodes can have more than two children.
- **Tree to Doubly Linked List:** Convert a tree to a doubly linked list in-place.
- **Ancestor/Descendant Queries:**
	- Find all ancestors/descendants of a node
	- Binary Lifting (advanced, for repeated ancestor queries)
- **Tree Diameter Variants:**
	- Diameter not passing through root
	- Longest univalue path
- **Zigzag/Spiral Level Order Traversal**
- **Vertical/Diagonal Order Traversal**
- **Flatten Binary Tree to Linked List**
- **Count Nodes in Complete Binary Tree (O(log^2 n))**

## Tree Problem-Solving Strategies
- Draw the tree for small examples.
- Use recursion for most tree problems, but know iterative approaches for interview follow-ups.
- Use a stack for iterative DFS, a queue for BFS.
- For parent pointers, use a HashMap or modify the node structure.
- For path problems, pass path/accumulated values as parameters or use backtracking.
- For subtree/duplicate subtree, use serialization or hashing.
- For construction, understand how preorder/inorder/postorder relate to tree structure.
- Always clarify if the tree is binary, BST, balanced, complete, etc.

---

## Final Interview Tips
- Practice writing code for all traversals and common patterns from scratch.
- Be ready to explain your recursion and base cases.
- Know how to optimize for space (Morris traversal, iterative methods).
- Be able to handle edge cases: empty tree, single node, skewed tree, duplicate values.
- Communicate your thought process and draw diagrams if allowed.

---
---

## Tree Operations: Insertion, Deletion, Search

### Insertion (BST Example)
```java
TreeNode insert(TreeNode root, int val) {
	if (root == null) return new TreeNode(val);
	if (val < root.val) root.left = insert(root.left, val);
	else root.right = insert(root.right, val);
	return root;
}
```

### Deletion (BST Example)
```java
TreeNode delete(TreeNode root, int key) {
	if (root == null) return null;
	if (key < root.val) root.left = delete(root.left, key);
	else if (key > root.val) root.right = delete(root.right, key);
	else {
		if (root.left == null) return root.right;
		if (root.right == null) return root.left;
		TreeNode minNode = getMin(root.right);
		root.val = minNode.val;
		root.right = delete(root.right, minNode.val);
	}
	return root;
}
TreeNode getMin(TreeNode node) {
	while (node.left != null) node = node.left;
	return node;
}
```

### Search (BST Example)
```java
boolean search(TreeNode root, int val) {
	if (root == null) return false;
	if (root.val == val) return true;
	return val < root.val ? search(root.left, val) : search(root.right, val);
}
```

---

## Generic DFS and BFS Templates

### DFS (Recursive, Generic)
```java
void dfs(TreeNode node) {
	if (node == null) return;
	// process node
	for (TreeNode child : children(node)) {
		dfs(child);
	}
}
```

### DFS (Iterative, Generic)
```java
void dfs(TreeNode root) {
	if (root == null) return;
	Stack<TreeNode> stack = new Stack<>();
	stack.push(root);
	while (!stack.isEmpty()) {
		TreeNode node = stack.pop();
		// process node
		for (TreeNode child : children(node)) {
			stack.push(child);
		}
	}
}
```

### BFS (Iterative, Generic)
```java
void bfs(TreeNode root) {
	if (root == null) return;
	Queue<TreeNode> queue = new LinkedList<>();
	queue.offer(root);
	while (!queue.isEmpty()) {
		TreeNode node = queue.poll();
		// process node
		for (TreeNode child : children(node)) {
			queue.offer(child);
		}
	}
}
```

### BFS (Recursive, Generic)
```java
void bfs(List<TreeNode> currentLevel) {
	if (currentLevel.isEmpty()) return;
	List<TreeNode> nextLevel = new ArrayList<>();
	for (TreeNode node : currentLevel) {
		// process node
		for (TreeNode child : children(node)) {
			nextLevel.add(child);
		}
	}
	bfs(nextLevel);
}
```

---

## Diameter, Height, and Depth of a Tree

- **Height of a Tree:**
  - The number of edges on the longest path from the root to a leaf.
  - Height of a leaf node is 0. Height of an empty tree is -1 or 0 (by convention).
  - Java code:
	```java
	int height(TreeNode root) {
		if (root == null) return -1; // or 0, depending on convention
		return 1 + Math.max(height(root.left), height(root.right));
	}
	```

- **Depth of a Node:**
  - The number of edges from the root to the node.
  - Depth of root is 0.
  - Java code:
	```java
	int depth(TreeNode root, TreeNode target) {
		if (root == null) return -1;
		if (root == target) return 0;
		int left = depth(root.left, target);
		if (left != -1) return left + 1;
		int right = depth(root.right, target);
		return right == -1 ? -1 : right + 1;
	}
	```

- **Diameter of a Tree:**
  - The length of the longest path between any two nodes in the tree (number of edges).
  - May or may not pass through the root.
  - Java code:
	```java
	int diameter = 0;
	int diameterOfBinaryTree(TreeNode root) {
		height(root);
		return diameter;
	}
	int height(TreeNode node) {
		if (node == null) return -1;
		int left = height(node.left);
		int right = height(node.right);
		diameter = Math.max(diameter, left + right + 2);
		return 1 + Math.max(left, right);
	}
	```
---
# Tree Traversal Templates & Key Concepts

## Traversal Orders
- **Preorder (Root-Left-Right)**: Visit root, then left subtree, then right subtree.
- **Inorder (Left-Root-Right)**: Visit left subtree, then root, then right subtree.
- **Postorder (Left-Right-Root)**: Visit left subtree, then right subtree, then root.
- **Level Order (Breadth-First)**: Visit nodes level by level from top to bottom.

---

## Recursive Traversal Templates

### Preorder (Recursive)
```java
void preorder(TreeNode root) {
	if (root == null) return;
	visit(root);
	preorder(root.left);
	preorder(root.right);
}
```

### Inorder (Recursive)
```java
void inorder(TreeNode root) {
	if (root == null) return;
	inorder(root.left);
	visit(root);
	inorder(root.right);
}
```

### Postorder (Recursive)
```java
void postorder(TreeNode root) {
	if (root == null) return;
	postorder(root.left);
	postorder(root.right);
	visit(root);
}
```

### Level Order (BFS, Recursive)
```java
// Usually implemented iteratively, but can be done recursively with a helper
void levelOrder(TreeNode root) {
	List<List<Integer>> result = new ArrayList<>();
	levelHelper(root, 0, result);
}
void levelHelper(TreeNode node, int level, List<List<Integer>> result) {
	if (node == null) return;
	if (result.size() == level) result.add(new ArrayList<>());
	result.get(level).add(node.val);
	levelHelper(node.left, level + 1, result);
	levelHelper(node.right, level + 1, result);
}
```

---

## Iterative Traversal Templates

### Preorder (Iterative)
```java
void preorder(TreeNode root) {
	if (root == null) return;
	Stack<TreeNode> stack = new Stack<>();
	stack.push(root);
	while (!stack.isEmpty()) {
		TreeNode node = stack.pop();
		visit(node);
		if (node.right != null) stack.push(node.right);
		if (node.left != null) stack.push(node.left);
	}
}
```

### Inorder (Iterative)
```java
void inorder(TreeNode root) {
	Stack<TreeNode> stack = new Stack<>();
	TreeNode curr = root;
	while (curr != null || !stack.isEmpty()) {
		while (curr != null) {
			stack.push(curr);
			curr = curr.left;
		}
		curr = stack.pop();
		visit(curr);
		curr = curr.right;
	}
}
```

### Postorder (Iterative)
```java
void postorder(TreeNode root) {
	Stack<TreeNode> stack = new Stack<>();
	TreeNode lastVisited = null;
	TreeNode curr = root;
	while (curr != null || !stack.isEmpty()) {
		if (curr != null) {
			stack.push(curr);
			curr = curr.left;
		} else {
			TreeNode peek = stack.peek();
			if (peek.right != null && lastVisited != peek.right) {
				curr = peek.right;
			} else {
				visit(peek);
				lastVisited = stack.pop();
			}
		}
	}
}
```

### Level Order (BFS, Iterative)
```java
void levelOrder(TreeNode root) {
	if (root == null) return;
	Queue<TreeNode> queue = new LinkedList<>();
	queue.offer(root);
	while (!queue.isEmpty()) {
		TreeNode node = queue.poll();
		visit(node);
		if (node.left != null) queue.offer(node.left);
		if (node.right != null) queue.offer(node.right);
	}
}
```

---

## Important Terms
- **DFS (Depth-First Search):** Explores as far as possible along each branch before backtracking. Includes preorder, inorder, and postorder traversals.
- **BFS (Breadth-First Search):** Explores all neighbors at the present depth before moving on to nodes at the next depth level. Level order traversal is BFS.
- **Height of Tree:** The number of edges on the longest path from the root to a leaf.
- **Depth of Node:** The number of edges from the root to the node.
- **Leaf Node:** A node with no children.
- **Parent/Child/Sibling:** Standard family relationships in trees.
- **Balanced Tree:** A tree where the heights of the two child subtrees of any node differ by at most one.
- **Binary Search Tree (BST):** A binary tree where for every node, left child < node < right child.
- **Complete Binary Tree:** All levels are completely filled except possibly the last, which is filled from left to right.
- **Full Binary Tree:** Every node has 0 or 2 children.
- **Perfect Binary Tree:** All internal nodes have two children and all leaves are at the same level.

---

## Time Complexity in Trees
- **Traversal (any order):** O(n), where n is the number of nodes.
- **Insert/Delete/Search in BST:** O(h), where h is the height of the tree (O(log n) for balanced BST, O(n) for skewed).
- **Balanced BST operations (AVL, Red-Black):** O(log n).
- **Heap operations (insert, delete):** O(log n).
- **Level order traversal:** O(n).

---

## Interview Tips
- Know both recursive and iterative versions of traversals.
- Be able to write DFS and BFS for trees and graphs.
- Understand the difference between tree and graph traversals (no cycles in trees).
- Practice problems involving subtree, path sum, lowest common ancestor, serialization/deserialization, and tree construction from traversals.
