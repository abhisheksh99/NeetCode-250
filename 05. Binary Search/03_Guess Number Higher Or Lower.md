# 374. Guess Number Higher or Lower
**Easy**

## Problem Statement
We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I will tell you whether the number I picked is higher or lower than your guess.

You call a pre-defined API `int guess(int num)`, which returns:
- -1: The number I picked is lower than your guess.
- 1: The number I picked is higher than your guess.
- 0: Your guess is correct.

Return the number that I picked.

## Examples
### Example 1:
- Input: n = 10, pick = 6
- Output: 6

### Example 2:
- Input: n = 1, pick = 1
- Output: 1

## Constraints
- 1 <= n <= 2^31 - 1
- 1 <= pick <= n

## Solution
```java
/** 
 * Forward declaration of guess API.
 * @param  num   your guess
 * @return      -1 if num is higher than the picked number
 *              1 if num is lower than the picked number
 *              otherwise return 0
 * int guess(int num);
 */

public class Solution extends GuessGame {
	public int guessNumber(int n) {
		int l = 1, r = n;
		while (true) {
			int m = l + (r - l) / 2;
			int res = guess(m);
			if (res > 0) {
				l = m + 1;
			} else if (res < 0) {
				r = m - 1;
			} else {
				return m;
			}
		}
	}
}
```

Approach

- Use binary search between 1 and n.
- At each step, guess the middle number and use the API to check if it's correct, too high, or too low.
- Adjust the search bounds based on the API response.
- Repeat until the correct number is found.

Time Complexity

O(log n), where n is the upper bound.

Space Complexity

O(1), as the search is performed in-place.
