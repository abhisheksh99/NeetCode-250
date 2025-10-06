# Linked List: Concepts, Template, and Patterns

## What is a Linked List?
A linked list is a linear data structure where each element (node) contains a value and a reference (pointer) to the next node in the sequence. It allows efficient insertion and deletion of elements at any position.

## Types of Linked Lists
- **Singly Linked List:** Each node points to the next node.
- **Doubly Linked List:** Each node points to both the next and previous nodes.
- **Circular Linked List:** The last node points back to the head.

## Common Operations
- **Insertion:** Add a node at the head, tail, or any position.
- **Deletion:** Remove a node by value or position.
- **Search:** Find a node by value.
- **Traversal:** Visit all nodes from head to tail.

## Java Linked List Node Template
```java
// Definition for singly-linked list node
class ListNode {
	int val;
	ListNode next;
	ListNode(int val) { this.val = val; }
}
```

## General Linked List Template (Iterative)
```java
// Traverse a linked list
ListNode curr = head;
while (curr != null) {
	// Process curr.val
	curr = curr.next;
}

// Insert at head
ListNode newNode = new ListNode(val);
newNode.next = head;
head = newNode;

// Insert at tail
ListNode curr = head;
while (curr.next != null) {
	curr = curr.next;
}
curr.next = new ListNode(val);

// Delete a node (given value)
ListNode dummy = new ListNode(-1);
dummy.next = head;
ListNode prev = dummy, curr = head;
while (curr != null) {
	if (curr.val == target) {
		prev.next = curr.next;
		break;
	}
	prev = curr;
	curr = curr.next;
}
head = dummy.next;
```

## Time and Space Complexity
- **Insertion at head:** O(1)
- **Insertion at tail:** O(n) (unless tail pointer is maintained)
- **Deletion:** O(n) (need to find the node)
- **Search:** O(n)
- **Space:** O(n) for n nodes

## Generic Patterns for Linked List Problems
- **Dummy Node Pattern:** Use a dummy node to simplify edge cases (especially for head insertion/deletion).
- **Two Pointer Pattern:** Use slow and fast pointers for cycle detection, finding the middle, etc.
- **Reversal Pattern:** Reverse a linked list iteratively or recursively.
- **Recursive Pattern:** Many problems can be solved recursively (e.g., reverse, merge, etc.).
- **In-place Modification:** Change pointers directly to avoid extra space.

### Example: In-place Removal of Elements (Remove all nodes with a given value)
```java
ListNode dummy = new ListNode(-1);
dummy.next = head;
ListNode prev = dummy, curr = head;
while (curr != null) {
	if (curr.val == target) {
		prev.next = curr.next; // Remove curr
	} else {
		prev = curr;
	}
	curr = curr.next;
}
head = dummy.next;
```

### Example: In-place Merge of Two Sorted Lists
```java
ListNode dummy = new ListNode(-1);
ListNode tail = dummy;
while (l1 != null && l2 != null) {
	if (l1.val < l2.val) {
		tail.next = l1;
		l1 = l1.next;
	} else {
		tail.next = l2;
		l2 = l2.next;
	}
	tail = tail.next;
}
tail.next = (l1 != null) ? l1 : l2;
head = dummy.next;
```

### Example: Two Pointer (Floyd's Cycle Detection)
```java
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
	slow = slow.next;
	fast = fast.next.next;
	if (slow == fast) {
		// Cycle detected
	}
}
```

### Example: Reverse a Linked List (Iterative)
```java
ListNode prev = null, curr = head;
while (curr != null) {
	ListNode nextTemp = curr.next;
	curr.next = prev;
	prev = curr;
	curr = nextTemp;
}
head = prev;
```
