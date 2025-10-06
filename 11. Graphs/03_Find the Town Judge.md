# 997. Find the Town Judge

## Problem Statement
In a town, there are `n` people labeled from `1` to `n`. There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:
1. The town judge trusts nobody.
2. Everybody (except for the town judge) trusts the town judge.
3. There is exactly one person that satisfies properties 1 and 2.

You are given an array `trust` where `trust[i] = [a_i, b_i]` representing that the person labeled `a_i` trusts the person labeled `b_i`.

Return the label of the town judge if the town judge exists and can be identified, or return `-1` otherwise.

### Examples

**Example 1:**
```
Input: n = 2, trust = [[1,2]]
Output: 2
Explanation: Person 1 trusts person 2. Person 2 trusts nobody.
```

**Example 2:**
```
Input: n = 3, trust = [[1,3],[2,3]]
Output: 3
Explanation: Person 1 and person 2 trust person 3. Person 3 trusts nobody.
```

**Example 3:**
```
Input: n = 3, trust = [[1,3],[2,3],[3,1]]
Output: -1
Explanation: Person 3 trusts person 1, so they cannot be the judge.
```

### Constraints:
- `1 <= n <= 1000`
- `0 <= trust.length <= 10^4`
- `trust[i].length == 2`
- All the pairs of `trust` are unique.
- `a_i != b_i`
- `1 <= a_i, b_i <= n`

## Solution
```java
class Solution {
    public int findJudge(int n, int[][] trust) {
        // Edge case: If there's only one person and no trust relationships, they are the judge
        if (trust.length == 0 && n == 1) return 1;
        
        // Initialize trust scores array (1-based indexing)
        int[] trustScores = new int[n + 1];
        
        // Calculate trust scores
        for (int[] pair : trust) {
            trustScores[pair[0]]--; // person trusts someone
            trustScores[pair[1]]++; // person is trusted by someone
        }
        
        // Check for the judge
        for (int i = 1; i <= n; i++) {
            // Judge is trusted by everyone else (n-1 people) and trusts nobody
            if (trustScores[i] == n - 1) {
                return i;
            }
        }
        
        return -1;
    }
}
```

## Approach
1. **Edge Case Handling**: If there's only one person and no trust relationships, that person is the judge.
2. **Trust Score Calculation**:
   - Create an array to track trust scores (1-based indexing).
   - For each trust relationship `[a, b]`:
     - Decrease the score of `a` (since they trust someone).
     - Increase the score of `b` (since they are trusted by someone).
3. **Judge Identification**:
   - The judge will have a trust score of `n-1` (trusted by everyone else and trusts nobody).
   - If no such person exists, return -1.

## Complexity
- **Time Complexity**: O(E + n), where E is the number of trust relationships (edges) and n is the number of people (nodes).
- **Space Complexity**: O(n) for the trust scores array.

## Edge Cases
- Single person with no trust relationships (should return 1).
- No judge exists (should return -1).
- Multiple people with the same trust score but no one meets the judge criteria.
- Maximum constraints (n=1000, trust.length=10^4).
- Empty trust array with n > 1 (no judge exists).

## Key Insights
- The judge must be trusted by everyone else (n-1 people) and trust nobody.
- We can represent this with a trust score where:
  - Each trust relationship `[a, b]` means `a` loses a point and `b` gains a point.
- The judge will be the only one with a trust score of `n-1`.
- This approach efficiently finds the judge in linear time with constant space (relative to n).