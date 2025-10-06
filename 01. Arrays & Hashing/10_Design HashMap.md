# 706. Design HashMap
**Easy**

## Problem Statement
Design a HashMap without using any built-in hash table libraries.
Implement the `MyHashMap` class:
- `void put(key, value)` Inserts a (key, value) pair into the HashMap. If the key already exists, update the value.
- `int get(key)` Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key.
- `void remove(key)` Removes the mapping of the specified value key if this map contains a mapping for the key.

## Examples
### Example 1:
```
Input:
["MyHashMap","put","put","get","get","put","get","remove","get"]
 [[],[1,1],[2,2],[1],[3],[2,1],[2],[2],[2]]
Output:
[null,null,null,1,-1,null,1,null,-1]
```

## Constraints
- 0 <= key, value <= 10^6
- At most 10^4 calls will be made to `put`, `get`, and `remove`.

## Solution
```java
public class MyHashMap {
	private int[] map;

	public MyHashMap() {
		map = new int[1000001];
		Arrays.fill(map, -1);
	}

	public void put(int key, int value) {
		map[key] = value;
	}

	public int get(int key) {
		return map[key];
	}

	public void remove(int key) {
		map[key] = -1;
	}
}
```

Approach

- Use an integer array of size 1,000,001 to store key-value pairs directly.
- Initialize all values to -1 to represent empty slots.
- To put a key-value pair, set `map[key] = value`.
- To get a value, return `map[key]` (returns -1 if not set).
- To remove a key, set `map[key] = -1`.
- Notes: This approach is efficient for the given constraints and avoids hash collisions.

Time Complexity

O(1) for all operations (put, get, remove).

Space Complexity

O(1), since the array size is fixed and does not depend on the number of operations.
