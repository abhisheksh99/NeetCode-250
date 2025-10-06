# Binary Search Templates (Java)

## Standard Binary Search
```java
// Returns the index of target if found, else -1
int binarySearch(int[] arr, int target) {
	int left = 0, right = arr.length - 1;
	while (left <= right) {
		int mid = left + (right - left) / 2;
		if (arr[mid] == target) {
			return mid;
		} else if (arr[mid] < target) {
			left = mid + 1;
		} else {
			right = mid - 1;
		}
	}
	return -1;
}
```

## Modified Binary Search (Lower/Upper Bound)
```java
// Lower Bound: First index where arr[i] >= target
int lowerBound(int[] arr, int target) {
	int left = 0, right = arr.length;
	while (left < right) {
		int mid = left + (right - left) / 2;
		if (arr[mid] < target) {
			left = mid + 1;
		} else {
			right = mid;
		}
	}
	return left;
}

// Upper Bound: First index where arr[i] > target
int upperBound(int[] arr, int target) {
	int left = 0, right = arr.length;
	while (left < right) {
		int mid = left + (right - left) / 2;
		if (arr[mid] <= target) {
			left = mid + 1;
		} else {
			right = mid;
		}
	}
	return left;
}
```

## Time and Space Complexity
- **Time Complexity:** O(log n) for all binary search variants.
- **Space Complexity:** O(1) (iterative), O(log n) if implemented recursively due to call stack.
