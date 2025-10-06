# 744. Find Smallest Letter Greater Than Target

## Problem Statement
Given a characters array `letters` that is sorted in non-decreasing order and a character `target`, return the smallest character in the array that is larger than the target. If no such character exists, return the first character in the array.

## Examples

### Example 1:
```
Input: letters = ["c","f","j"], target = "a"
Output: "c"
Explanation: The smallest character in the array that is larger than 'a' is 'c'.
```

### Example 2:
```
Input: letters = ["c","f","j"], target = "c"
Output: "f"
Explanation: The smallest character in the array that is larger than 'c' is 'f'.
```

### Example 3:
```
Input: letters = ["x","x","y","y"], target = "z"
Output: "x"
Explanation: There are no characters in the array that is larger than 'z', so we return letters[0].
```

## Approach
We can solve this problem using binary search since the array is sorted. The key insight is to find the first character in the array that is greater than the target. If no such character exists, we return the first character in the array.

## Solution Code
```java
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int left = 0;
        int right = letters.length - 1;
        
        // If target is greater than or equal to the last character, return the first character
        if (target >= letters[right]) {
            return letters[0];
        }
        
        // Binary search to find the smallest character greater than target
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (letters[mid] <= target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        // After the loop, left will point to the first character greater than target
        return letters[left];
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(log n) - We perform a binary search on the array.
- **Space Complexity**: O(1) - We use a constant amount of extra space.

## Key Insights
1. **Binary Search**: Since the array is sorted, we can use binary search to find the target character efficiently.
2. **Edge Case Handling**: If the target is greater than or equal to the last character, we return the first character in the array.
3. **Loop Invariant**: The loop maintains the invariant that the answer is within the range [left, right].
4. **Termination**: The loop terminates when left > right, and left points to the first character greater than the target.

## Edge Cases
1. Target is less than all characters in the array
2. Target is greater than or equal to the last character in the array
3. Array contains only one character
4. Array contains duplicate characters
5. Target is exactly equal to a character in the array

## Follow-up
How would you modify the solution if the array contains both uppercase and lowercase letters, and the comparison should be case-insensitive?