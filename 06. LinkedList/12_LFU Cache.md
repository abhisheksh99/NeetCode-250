# 460. LFU Cache
**Hard**

## Problem Statement
Design and implement a data structure for a Least Frequently Used (LFU) cache.

Implement the LFUCache class:
- LFUCache(int capacity) Initializes the object with the capacity of the data structure.
- int get(int key) Gets the value of the key if the key exists in the cache. Otherwise, returns -1.
- void put(int key, int value) Update the value of the key if present, or inserts the key if not present. When the cache reaches its capacity, it should invalidate and remove the least frequently used key before inserting a new item. For this problem, when there is a tie (i.e., two or more keys with the same frequency), the least recently used key is evicted.

The functions get and put must each run in O(1) average time complexity.

## Examples
### Example 1:
- Input: ["LFUCache","put","put","get","put","get","get","put","get","get","get"] [[2],[1,1],[2,2],[1],[3,3],[2],[3],[4,4],[1],[3],[4]]
- Output: [null,null,null,1,null,-1,3,null,-1,3,4]

## Constraints
- 0 <= capacity <= 10^4
- 0 <= key <= 10^5
- 0 <= value <= 10^9
- At most 2 * 10^5 calls will be made to get and put.

## Solution
```java
class LFUCache {
    private class Node {
        int key, value, freq;
        Node prev, next;
        Node(int k, int v) {
            key = k; value = v; freq = 1;
        }
    }
    private class DoublyLinkedList {
        Node head, tail;
        int size;
        DoublyLinkedList() {
            head = new Node(0, 0);
            tail = new Node(0, 0);
            head.next = tail;
            tail.prev = head;
            size = 0;
        }
        void addNode(Node node) {
            node.next = head.next;
            node.prev = head;
            head.next.prev = node;
            head.next = node;
            size++;
        }
        void removeNode(Node node) {
            node.prev.next = node.next;
            node.next.prev = node.prev;
            size--;
        }
        Node removeTail() {
            if (size > 0) {
                Node node = tail.prev;
                removeNode(node);
                return node;
            }
            return null;
        }
        boolean isEmpty() {
            return size == 0;
        }
    }
    private final int capacity;
    private int minFreq;
    private Map<Integer, Node> cache;
    private Map<Integer, DoublyLinkedList> freqMap;
    public LFUCache(int capacity) {
        this.capacity = capacity;
        minFreq = 0;
        cache = new HashMap<>();
        freqMap = new HashMap<>();
    }
    public int get(int key) {
        if (!cache.containsKey(key)) return -1;
        Node node = cache.get(key);
        updateFreq(node);
        return node.value;
    }
    public void put(int key, int value) {
        if (capacity == 0) return;
        if (cache.containsKey(key)) {
            Node node = cache.get(key);
            node.value = value;
            updateFreq(node);
        } else {
            if (cache.size() == capacity) {
                DoublyLinkedList minFreqList = freqMap.get(minFreq);
                Node toRemove = minFreqList.removeTail();
                cache.remove(toRemove.key);
            }
            Node newNode = new Node(key, value);
            cache.put(key, newNode);
            minFreq = 1;
            freqMap.computeIfAbsent(1, k -> new DoublyLinkedList()).addNode(newNode);
        }
    }
    private void updateFreq(Node node) {
        int freq = node.freq;
        DoublyLinkedList oldList = freqMap.get(freq);
        oldList.removeNode(node);
        if (freq == minFreq && oldList.isEmpty()) {
            minFreq++;
        }
        node.freq++;
        freqMap.computeIfAbsent(node.freq, k -> new DoublyLinkedList()).addNode(node);
    }
}
```

Approach
- Use a HashMap to store key-node pairs for O(1) access.
- Use a HashMap to map frequencies to doubly linked lists of nodes with that frequency.
- Each node stores its key, value, and frequency.
- On get/put, update the node's frequency and move it to the correct list.
- When capacity is reached, evict the least frequently used and least recently used node.

Time Complexity
- get, put: O(1) average time.

Space Complexity
- O(capacity), for the HashMaps and linked lists.
