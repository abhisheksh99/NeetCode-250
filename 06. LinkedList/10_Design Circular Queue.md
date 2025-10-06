# 622. Design Circular Queue
**Medium**

## Problem Statement
Design your implementation of the circular queue. The circular queue is a linear data structure in which the operations are performed based on FIFO (First In First Out) principle and the last position is connected back to the first position to make a circle. It is also known as "Ring Buffer".

Implement the MyCircularQueue class:
- MyCircularQueue(k) Initializes the object with the size of the queue to be k.
- int Front() Gets the front item from the queue. If the queue is empty, return -1.
- int Rear() Gets the last item from the queue. If the queue is empty, return -1.
- boolean enQueue(int value) Inserts an element into the circular queue. Return true if the operation is successful.
- boolean deQueue() Deletes an element from the circular queue. Return true if the operation is successful.
- boolean isEmpty() Checks whether the circular queue is empty or not.
- boolean isFull() Checks whether the circular queue is full or not.

You must solve the problem without using the built-in Queue library in Java.

## Examples
### Example 1:
- Input: ["MyCircularQueue","enQueue","enQueue","enQueue","enQueue","Rear","isFull","deQueue","enQueue","Rear"] [[3],[1],[2],[3],[4],[],[],[],[4],[]]
- Output: [null,true,true,true,false,3,true,true,true,4]

## Constraints
- 1 <= k <= 1000
- 0 <= value <= 1000
- At most 3000 calls will be made to enQueue, deQueue, Front, Rear, isEmpty, and isFull.

## Solution
```java
public class MyCircularQueue {
    private List<Integer> queue;
    private int capacity;

    public MyCircularQueue(int k) {
        queue = new ArrayList<>();
        capacity = k;
    }

    public boolean enQueue(int value) {
        if (queue.size() == capacity) {
            return false;
        }
        queue.add(value);
        return true;
    }

    public boolean deQueue() {
        if (queue.isEmpty()) {
            return false;
        }
        queue.remove(0);
        return true;
    }

    public int Front() {
        return queue.isEmpty() ? -1 : queue.get(0);
    }

    public int Rear() {
        return queue.isEmpty() ? -1 : queue.get(queue.size() - 1);
    }

    public boolean isEmpty() {
        return queue.isEmpty();
    }

    public boolean isFull() {
        return queue.size() == capacity;
    }
}
```

Approach
- Use an ArrayList to store the elements and a variable to track the capacity.
- For enQueue, add to the end if not full.
- For deQueue, remove from the front if not empty.
- Front and Rear return the first and last elements, or -1 if empty.
- isEmpty and isFull check the size against 0 and capacity.

Time Complexity
- enQueue, deQueue: O(n) (due to ArrayList remove at index 0)
- Front, Rear, isEmpty, isFull: O(1)

Space Complexity
- O(k), where k is the capacity of the queue.
