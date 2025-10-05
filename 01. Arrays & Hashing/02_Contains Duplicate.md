217. Contains Duplicate

Easy

Problem Statement

Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

Examples

Example 1:





Input: nums = [1,2,3,1]



Output: true

Example 2:





Input: nums = [1,2,3,4]



Output: false

Example 3:





Input: nums = [1,1,1,3,3,4,3,2,4,2]



Output: true

Constraints





1 <= nums.length <= 10^5



-10^9 <= nums[i] <= 10^9

Solution

class Solution {
    public boolean hasDuplicate(int[] nums) {
        HashSet<Integer> seen = new HashSet<>();
        for(int num : nums) {
            if(seen.contains(num)) {
                return true;
            }
            seen.add(num);
        }
        return false;
    }
}

Approach





Initialize a HashSet: Create a HashSet called seen to store unique elements.



Iterate through the array: Loop through each element in the nums array.



Check for duplicates: For each element, check if it already exists in the HashSet using contains. If it does, return true as a duplicate is found.



Add to HashSet: If the element is not in the HashSet, add it to seen.



Return result: If the loop completes without finding duplicates, return false.

Time Complexity





O(n): We iterate through the array once, and HashSet operations (contains and add) are O(1) on average.

Space Complexity





O(n): In the worst case, the HashSet stores all elements of the array if there are no duplicates.