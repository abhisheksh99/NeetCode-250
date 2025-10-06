# Roman to Integer

**Difficulty:** Easy

## Problem Statement  
Given a Roman numeral string, convert it to an integer.

Roman numerals are represented by seven different symbols:  
| Symbol | Value |  
|--------|-------|  
| I      | 1     |  
| V      | 5     |  
| X      | 10    |  
| L      | 50    |  
| C      | 100   |  
| D      | 500   |  
| M      | 1000  |  

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five, we subtract it making four. The same principle applies to the number nine, which is written as `IX`.  

---

## Example  
Input:  
s = "MCMXCIV"


Output:1994


Explanation:  
M = 1000, CM = 900, XC = 90, IV = 4  
So, 1000 + 900 + 90 + 4 = 1994

---

## Constraints  
- `1 <= s.length <= 15`  
- `s` contains only the characters ('I', 'V', 'X', 'L', 'C', 'D', 'M').  
- It is guaranteed that `s` is a valid Roman numeral in the range [1, 3999].

---

## Approach  
- Create a map of Roman characters to their integer values.  
- Iterate through the string:  
  - If the current symbol is smaller than the next symbol, subtract its value from the result.  
  - Otherwise, add its value.  
- This captures the subtractive notation used in Roman numerals (like IV, IX, etc.).  

---

## Time and Space Complexity  
- **Time Complexity:** O(n), where n is the length of the string `s`.  
- **Space Complexity:** O(1), as the map size is fixed and constant.

---

## Code

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int romanToInt(String s) {
        Map<Character, Integer> roman = new HashMap<>();
        roman.put('I', 1); 
        roman.put('V', 5);
        roman.put('X', 10); 
        roman.put('L', 50);
        roman.put('C', 100); 
        roman.put('D', 500);
        roman.put('M', 1000);

        int res = 0;
        for (int i = 0; i < s.length(); i++) {
            if (i + 1 < s.length() && roman.get(s.charAt(i)) < roman.get(s.charAt(i + 1))) {
                res -= roman.get(s.charAt(i));
            } else {
                res += roman.get(s.charAt(i));
            }
        }
        return res;
    }
}
```
