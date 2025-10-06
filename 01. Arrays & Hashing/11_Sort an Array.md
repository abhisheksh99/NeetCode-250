# 912. Sort an Array
**Medium**

## Problem Statement
Given an array of integers `nums`, sort the array in ascending order and return it.
You must solve the problem without using any built-in sort functions.

## Examples
### Example 1:
- Input: nums = [5,2,3,1]
- Output: [1,2,3,5]

### Example 2:
- Input: nums = [5,1,1,2,0,0]
- Output: [0,0,1,1,2,5]

## Constraints
- 1 <= nums.length <= 5 * 10^4
- -5 * 10^4 <= nums[i] <= 5 * 10^4

## Solution
```java
class Solution {
	public void merge(int nums[], int s, int m, int e) {
		int n1 = m - s + 1;
		int n2 = e - m;
        
		int arr1[] = new int[n1];
		int arr2[] = new int[n2];

		// Copy data to temporary arrays
		for (int i = 0; i < n1; i++) {
			arr1[i] = nums[s + i];
		}
		for (int i = 0; i < n2; i++) {
			arr2[i] = nums[m + 1 + i];
		}

		// Merge the two subarrays
		int i = 0, j = 0, k = s;
		while (i < n1 && j < n2) {
			if (arr1[i] <= arr2[j]) {
				nums[k] = arr1[i];
				i++;
			} else {
				nums[k] = arr2[j];
				j++;
			}
			k++;
		}

		// Copy remaining elements of arr1[], if any
		while (i < n1) {
			nums[k] = arr1[i];
			i++;
			k++;
		}

		// Copy remaining elements of arr2[], if any
		while (j < n2) {
			nums[k] = arr2[j];
			j++;
			k++;
		}
	}

	public void mergeSort(int nums[], int s, int e) {
		if (s < e) {
			int m = s + (e - s) / 2; // Calculate middle index
			mergeSort(nums, s, m);
			mergeSort(nums, m + 1, e);
			merge(nums, s, m, e); // Merge the sorted halves
		}
	}

	public int[] sortArray(int[] nums) {
		mergeSort(nums, 0, nums.length - 1);
		return nums;
	}
}
```

Approach

- Use the Merge Sort algorithm to sort the array.
- Recursively divide the array into two halves until each subarray has one element.
- Merge the sorted subarrays by comparing elements and copying the smaller one into the result array.
- Continue merging until the entire array is sorted.
- Notes: Merge Sort is a stable, divide-and-conquer sorting algorithm.

Time Complexity

O(n log n), where n is the length of the array.

Space Complexity

O(n), due to the temporary arrays used during merging.
