# 347. Top K Frequent Elements
**Medium**

## Problem Statement
Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

## Examples
### Example 1:
- Input: nums = [1,1,1,2,2,3], k = 2
- Output: [1,2]

### Example 2:
- Input: nums = [1], k = 1
- Output: [1]

## Constraints
- 1 <= nums.length <= 10^5
- k is in the range [1, the number of unique elements in the array]
- It is guaranteed that the answer is unique.

## Solution
```java
class Solution {
	public int[] topKFrequent(int[] nums, int k) {
		if(k==nums.length){
			return nums;
		}
		Map<Integer,Integer> map = new HashMap<>();

		for(int num:nums){
			map.put(num,map.getOrDefault(num,0)+1);
		}

		Queue<Integer> heap = new PriorityQueue((a,b) -> map.get(a) - map.get(b));

		for(int n:map.keySet()){
			heap.add(n);
			if(heap.size()>k){
				heap.poll();
			}
		}

		int[] ans = new int[k];
		for(int i=0;i<k;i++){
			ans[i]=heap.poll();
		}
		return ans;
	}
}
```

Approach

- Count the frequency of each element in the array using a HashMap.
- Use a min-heap (PriorityQueue) to keep track of the top k frequent elements:
  - For each unique element, add it to the heap.
  - If the heap size exceeds k, remove the element with the lowest frequency.
- After processing all elements, extract the k most frequent elements from the heap.
- Notes: This approach ensures efficient retrieval of the k most frequent elements.

Time Complexity

O(n log k), where n is the length of the array (heap operations are log k).

Space Complexity

O(n), for the frequency map and heap.
