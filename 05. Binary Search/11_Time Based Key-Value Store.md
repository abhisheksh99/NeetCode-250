# 981. Time Based Key-Value Store
**Medium**

## Problem Statement
Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the value at a certain timestamp.

Implement the `TimeMap` class:
- `TimeMap()` Initializes the object of the data structure.
- `void set(String key, String value, int timestamp)` Stores the key and value, along with the given timestamp.
- `String get(String key, int timestamp)` Returns a value such that set was called previously with `timestamp_prev <= timestamp`. If there are multiple such values, it returns the one with the largest `timestamp_prev`. If there are no values, it returns an empty string.

## Examples
### Example 1:
- Input: ["TimeMap","set","get","get","set","get","get"], [[],["foo","bar",1],["foo",1],["foo",3],["foo","bar2",4],["foo",4],["foo",5]]
- Output: [null,null,"bar","bar",null,"bar2","bar2"]

## Constraints
- 1 <= key.length, value.length <= 100
- key and value consist of lowercase English letters and digits.
- 1 <= timestamp <= 10^7
- All the timestamps `timestamp` of set are strictly increasing.
- At most 2 * 10^5 calls will be made to set and get.

## Solution
```java
class TimeMap {
    // Map to store key-value pairs with timestamps
    private Map<String, TreeMap<Integer, String>> map;

    // Initialize the data structure with an empty HashMap
    public TimeMap() {
        map = new HashMap<>();
    }
    
    // Store key, value, and timestamp in the map
    public void set(String key, String value, int timestamp) {
        map.computeIfAbsent(key, k -> new TreeMap<>()).put(timestamp, value);
    }
    
    // Retrieve the most recent value for a key before or at the given timestamp
    public String get(String key, int timestamp) {
        TreeMap<Integer, String> treeMap = map.get(key);
        if (treeMap == null) {
            return "";
        }
        Map.Entry<Integer, String> entry = treeMap.floorEntry(timestamp);
        return entry == null ? "" : entry.getValue();
    }
}
```

Approach

- Use a HashMap to map each key to a TreeMap of timestamp-value pairs.
- For `set`, insert the value with its timestamp into the TreeMap for the key.
- For `get`, use `floorEntry` to find the value with the largest timestamp less than or equal to the given timestamp.
- If no such value exists, return an empty string.

Time Complexity

- `set`: O(log n) per operation (TreeMap insertion)
- `get`: O(log n) per operation (TreeMap floorEntry)
- n is the number of timestamps for a key.

Space Complexity

- O(N), where N is the total number of set operations (all key-value-timestamp triples stored).
