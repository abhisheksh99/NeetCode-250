# GCD of Strings

**Difficulty:** Easy

## Problem Statement  
Given two strings `str1` and `str2`, return the largest string `x` such that `x` divides both `str1` and `str2`.

A string `t` divides string `s` if and only if `s` is formed by concatenating `t` one or more times.

**Example:**  
Input: str1 = "ABABAB", str2 = "ABAB"  
Output: "AB"

**Constraints:**  
- 1 <= str1.length, str2.length <= 1000  
- str1 and str2 consist of uppercase English letters.

---

## Approach  
- Find the greatest common divisor (gcd) of the lengths of `str1` and `str2`.  
- Check if both `str1` and `str2` are made up of repetitions of the substring `str1.substring(0, gcd_length)`.  
- If both satisfy, return that substring; otherwise, return an empty string.

---

## Time and Space Complexity  
- **Time Complexity:** O(n + m), where n and m are lengths of the two strings (checking repeated patterns).  
- **Space Complexity:** O(n + m) due to substring operations and string comparisons.

```java
public class Solution {
    public String gcdOfStrings(String str1, String str2) {
        int g = gcd(str1.length(), str2.length());

        for (int i = 0; i < str1.length(); i++) {
            if (str1.charAt(i) != str1.charAt(i % g)) {
                return "";
            }
        }

        for (int i = 0; i < str2.length(); i++) {
            if (str2.charAt(i) != str1.charAt(i % g)) {
                return "";
            }
        }

        return str1.substring(0, g);
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```