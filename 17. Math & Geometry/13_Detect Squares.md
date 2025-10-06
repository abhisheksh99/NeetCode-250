
# Detect Squares

**Difficulty:** Medium

## Problem Statement  
Design a data structure that supports adding points and counting the number of axis-aligned squares that can be formed with a given point as one of the corners.

You need to implement two operations:

- `add(point)`: Adds a point `(x, y)` to the data structure. Points can be added multiple times.
- `count(point)`: Returns the number of squares that can be formed with the given point as one of the corners.

---

## Example Behavior

- After adding points `(3, 10)`, `(11, 2)`, and `(3, 2)`:
  - Counting squares with point `(11, 10)` returns **1**.
- Counting squares with point `(14, 8)` returns **0**.
- After adding point `(11, 2)` again (duplicate point):
  - Counting squares with point `(11, 10)` returns **2**.

---

## Constraints  
- Coordinates can be any integer values.  
- Points can be added multiple times.

---

## Approach  
- Maintain a list of all added points and a frequency map keyed by `"x@y"`.  
- To count squares with query point `(px, py)`:
  - Iterate over all points `(x, y)` already added.  
  - Check if `(x, y)` can be the diagonal corner of a square with `(px, py)`:  
    - `px != x`, `py != y`, and `abs(px - x) == abs(py - y)`.  
  - Count how many points exist at `(x, py)` and `(px, y)` using the frequency map.  
  - Multiply these counts and accumulate for total squares.

---

## Time and Space Complexity  
- **Add operation:** O(1)  
- **Count operation:** O(N), where N is the number of points added  
- **Space complexity:** O(N)

---

## Code

```java
import java.util.*;

public class CountSquares {
    private List<int[]> coordinates;
    private Map<String, Integer> counts;

    public CountSquares() {
        coordinates = new ArrayList<>();
        counts = new HashMap<>();
    }

    public void add(int[] point) {
        coordinates.add(point);
        String key = point[0] + "@" + point[1];
        counts.put(key, counts.getOrDefault(key, 0) + 1);
    }

    public int count(int[] point) {
        int sum = 0, px = point[0], py = point[1];
        for (int[] coordinate : coordinates) {
            int x = coordinate[0], y = coordinate[1];

            if (px == x || py == y || Math.abs(px - x) != Math.abs(py - y))
                continue;

            int count1 = counts.getOrDefault(x + "@" + py, 0);
            int count2 = counts.getOrDefault(px + "@" + y, 0);
            sum += count1 * count2;
        }
        return sum;
    }
}
