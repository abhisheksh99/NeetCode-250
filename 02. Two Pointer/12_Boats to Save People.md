# 881. Boats to Save People
**Medium**

## Problem Statement
You are given an array `people` where `people[i]` is the weight of the i-th person, and an integer `limit` representing the maximum weight a boat can carry.
Each boat carries at most two people at the same time, provided the sum of the weight of those people is at most `limit`.
Return the minimum number of boats to carry every given person.

## Examples
### Example 1:
- Input: people = [1,2], limit = 3
- Output: 1

### Example 2:
- Input: people = [3,2,2,1], limit = 3
- Output: 3

### Example 3:
- Input: people = [3,5,3,4], limit = 5
- Output: 4

## Constraints
- 1 <= people.length <= 5 * 10^4
- 1 <= people[i] <= limit <= 3 * 10^4

## Solution
```java
class Solution {
	public int numRescueBoats(int[] people, int limit) 
	{
		Arrays.sort(people);
		int l=0,r=people.length-1;
		int boats = 0;
		while(l<=r)
		{
			if (people[l] + people[r] <= limit)
				l++;  // pair lightest with heaviest
			r--; //heaviest person boards
			boats++;
		}

		return boats;
	}
}
```

Approach

- Sort the array of people's weights.
- Use two pointers: `l` at the start (lightest) and `r` at the end (heaviest).
- If the sum of the lightest and heaviest person is within the limit, pair them and move both pointers inward.
- Otherwise, the heaviest person goes alone; move the right pointer inward.
- Increment the boat count for each iteration.
- Repeat until all people are assigned to boats.

Time Complexity

O(n log n), where n is the number of people (due to sorting).

Space Complexity

O(1), as only a few pointers/variables are used (excluding sort space).
