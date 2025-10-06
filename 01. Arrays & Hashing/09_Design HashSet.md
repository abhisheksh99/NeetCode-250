# 705. Design HashSet
**Easy**

## Problem Statement
Design a HashSet without using any built-in hash table libraries.
Implement the `MyHashSet` class:
- `void add(key)` Inserts the value `key` into the HashSet.
- `bool contains(key)` Returns whether the value `key` exists in the HashSet or not.
- `void remove(key)` Removes the value `key` in the HashSet. If `key` does not exist in the HashSet, do nothing.

## Examples
### Example 1:
```
Input:
["MyHashSet","add","add","contains","contains","add","contains","remove","contains"]
 [[],[1],[2],[1],[3],[2],[2],[2],[2]]
Output:
[null,null,null,true,false,null,true,null,false]
```

## Constraints
- 0 <= key <= 10^6
- At most 10^4 calls will be made to `add`, `remove`, and `contains`.

## Solution
```java
public class MyHashSet {
	private boolean[] data;

	public MyHashSet() {
		data = new boolean[1000001];
	}

	public void add(int key) {
		data[key] = true;
	}

	public void remove(int key) {
		data[key] = false;
	}

	public boolean contains(int key) {
		return data[key];
	}
}
```

Approach

- Use a boolean array of size 1,000,001 to represent the presence of each possible key.
- To add a key, set the corresponding index in the array to true.
- To remove a key, set the corresponding index to false.
- To check if a key exists, return the value at the corresponding index.
- Notes: This approach is efficient for the given constraints and avoids hash collisions.

Time Complexity

O(1) for all operations (add, remove, contains).

Space Complexity

O(1), since the array size is fixed and does not depend on the number of operations.
