# 735. Asteroid Collision
**Medium**

## Problem Statement
We are given an array `asteroids` of integers representing asteroids in a row.
For each asteroid, the absolute value represents its size, and the sign represents its direction (positive means right, negative means left).
Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one explodes. If both are the same size, both explode. Two asteroids moving in the same direction will never meet.

## Examples
### Example 1:
- Input: asteroids = [5,10,-5]
- Output: [5,10]
- Explanation: The 10 and -5 collide resulting in 10. The 5 and 10 never collide.

### Example 2:
- Input: asteroids = [8,-8]
- Output: []
- Explanation: The 8 and -8 collide and both are destroyed.

### Example 3:
- Input: asteroids = [10,2,-5]
- Output: [10]

## Constraints
- 2 <= asteroids.length <= 10^4
- -1000 <= asteroids[i] <= 1000

## Solution
```java
class Solution {
	public int[] asteroidCollision(int[] asteroids) {
		Stack<Integer> stack = new Stack<>();

		for (int asteroid : asteroids) {
			boolean alive = true; // flag to check if current asteroid survives

			while (alive && asteroid < 0 && !stack.isEmpty() && stack.peek() > 0) {
				int top = stack.peek();

				if (top < -asteroid) {      // top explodes
					stack.pop();
				} else if (top == -asteroid) { // both explode
					stack.pop();
					alive = false;
				} else {                     // current explodes
					alive = false;
				}
			}

			if (alive) {
				stack.push(asteroid);
			}
		}

		// Convert stack to array
		int[] result = new int[stack.size()];
		for (int i = stack.size() - 1; i >= 0; i--) {
			result[i] = stack.pop();
		}

		return result;
	}
}
```

Approach

- Use a stack to simulate the collisions.
- For each asteroid:
  - If it is moving right (positive), push onto the stack.
  - If it is moving left (negative), check for collisions with the stack's top (right-moving asteroids).
  - Pop smaller right-moving asteroids, handle equal size (both explode), or stop if the left-moving asteroid explodes.
- After processing, the stack contains the surviving asteroids.

Time Complexity

O(n), where n is the number of asteroids.

Space Complexity

O(n), for the stack in the worst case (no collisions).
