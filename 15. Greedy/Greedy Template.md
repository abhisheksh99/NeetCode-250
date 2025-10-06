# Greedy Algorithms: Patterns & Templates (Java)

## Table of Contents
1. [Introduction to Greedy Algorithms](#introduction)
2. [When to Use Greedy Approach](#when-to-use)
3. [Common Greedy Patterns](#common-patterns)
4. [Templates](#templates)
5. [Time & Space Complexity](#complexity)
6. [Tips & Tricks](#tips)

## Introduction to Greedy Algorithms <a name="introduction"></a>
Greedy algorithms make the locally optimal choice at each step with the hope of finding a global optimum. They are simple to implement and often efficient, but don't always produce the optimal solution.

### Key Characteristics:
- Makes locally optimal choices at each step
- Never reconsiders previous choices
- Often requires a proof of correctness
- Typically involves sorting or priority queues

## When to Use Greedy Approach <a name="when-to-use"></a>
Greedy algorithms are often effective for:
- Optimization problems
- Problems where local optimal leads to global optimal
- Scheduling problems
- Minimum spanning tree (Prim's, Kruskal's)
- Shortest path (Dijkstra's)
- Huffman coding

## Common Greedy Patterns <a name="common-patterns"></a>

### 1. Activity Selection
```java
import java.util.*;

class Activity {
    int start, end;
    Activity(int s, int e) { start = s; end = e; }
}

public List<Activity> activitySelection(Activity[] activities) {
    // Sort activities by finish time
    Arrays.sort(activities, (a, b) -> a.end - b.end);
    
    List<Activity> selected = new ArrayList<>();
    selected.add(activities[0]);
    int lastFinish = activities[0].end;
    
    for (int i = 1; i < activities.length; i++) {
        if (activities[i].start >= lastFinish) {
            selected.add(activities[i]);
            lastFinish = activities[i].end;
        }
    }
    
    return selected;
}
```

### 2. Fractional Knapsack
```java
import java.util.*;

class Item {
    int value, weight;
    Item(int v, int w) { value = v; weight = w; }
}

public double fractionalKnapsack(Item[] items, int capacity) {
    // Sort items by value/weight ratio in descending order
    Arrays.sort(items, (a, b) -> 
        Double.compare((double)b.value/b.weight, (double)a.value/a.weight));
    
    double totalValue = 0.0;
    
    for (Item item : items) {
        if (capacity == 0) break;
        
        double take = Math.min(item.weight, capacity);
        totalValue += take * ((double)item.value / item.weight);
        capacity -= take;
    }
    
    return totalValue;
}
```

### 3. Interval Scheduling
```java
import java.util.*;

public int intervalScheduling(int[][] intervals) {
    if (intervals == null || intervals.length == 0) return 0;
    
    // Sort by end time
    Arrays.sort(intervals, (a, b) -> a[1] - b[1]);
    
    int count = 1;
    int end = intervals[0][1];
    
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] >= end) {
            count++;
            end = intervals[i][1];
        }
    }
    
    return count;
}
```

## Templates <a name="templates"></a>

### Greedy Template (General)
```java
public ReturnType greedySolution(ElementType[] elements) {
    // 1. Sort or preprocess elements
    Arrays.sort(elements, (a, b) -> {
        // Define comparison logic
        return a.compareTo(b);
    });
    
    // 2. Initialize variables
    ReturnType result = initialValue;
    CurrentState current = initialState;
    
    // 3. Greedy selection
    for (ElementType element : elements) {
        if (isValid(current, element)) {
            // Make greedy choice
            current = update(current, element);
            result = updateResult(result, element);
        }
    }
    
    return result;
}
```

### Two Pointers Greedy
```java
public int twoPointersGreedy(int[] arr) {
    int left = 0, right = arr.length - 1;
    int result = 0;
    
    while (left <= right) {
        // Make greedy choice
        if (condition(arr[left], arr[right])) {
            // Process left
            left++;
        } else {
            // Process right
            right--;
        }
        
        // Update result
        result = updateResult(result, left, right);
    }
    
    return result;
}
```

## Time & Space Complexity <a name="complexity"></a>

| Pattern | Time Complexity | Space Complexity |
|---------|-----------------|------------------|
| Activity Selection | O(n log n) | O(1) or O(n) for result |
| Fractional Knapsack | O(n log n) | O(1) |
| Interval Scheduling | O(n log n) | O(1) or O(n) for result |
| Two Pointers | O(n) | O(1) |
| Greedy with Sorting | O(n log n) | O(log n) for sorting |
| Greedy with Heap | O(n log k) | O(k) where k is heap size |

## Tips & Tricks <a name="tips"></a>
1. **Sorting is Key**: Use `Arrays.sort()` with custom comparators for objects
2. **Use PriorityQueue**: For problems requiring a heap, Java's `PriorityQueue` is efficient
3. **Integer vs int**: Be mindful of autoboxing in performance-critical sections
4. **Common Patterns**:
   - Sort by end time/start time
   - Sort by value/weight ratio
   - Two pointers approach
5. **Edge Cases**:
   - Empty or null input
   - Single element
   - All elements same
   - Already sorted input
   - Reverse sorted input
6. **When Greedy Fails**:
   - Need to consider future choices
   - Local optimal doesn't lead to global optimal
   - Need to backtrack

## Common Problems
1. Jump Game I/II
2. Gas Station
3. Candy
4. Partition Labels
5. Valid Parenthesis String
6. Hand of Straights
7. Dota2 Senate
8. Merge Triplets to Form Target Triplet