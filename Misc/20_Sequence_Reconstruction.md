# 444. Sequence Reconstruction

## Problem Statement
Check whether the original sequence `org` can be uniquely reconstructed from the sequences in `seqs`. The `org` sequence is a permutation of the integers from 1 to n, with 1 ≤ n ≤ 10^4. Reconstruction means building a shortest common supersequence of the sequences in `seqs` that results in `org`.

### Examples

#### Example 1:
```
Input: org = [1,2,3], seqs = [[1,2],[1,3]]
Output: false
Explanation: [1,2,3] is not the only possible reconstruction. [1,3,2] is also a valid reconstruction.
```

#### Example 2:
```
Input: org = [1,2,3], seqs = [[1,2]]
Output: false
Explanation: The reconstructed sequence can only be [1,2], which is not a valid reconstruction.
```

#### Example 3:
```
Input: org = [1,2,3], seqs = [[1,2],[1,3],[2,3]]
Output: true
Explanation: The sequences [1,2], [1,3], and [2,3] can uniquely reconstruct the original sequence [1,2,3].
```

## Approach
This problem can be solved using topological sorting with the following steps:

1. **Build the Graph and In-degree Map**:
   - Create an adjacency list to represent the directed graph where an edge from `u` to `v` means `u` must appear before `v` in the reconstructed sequence.
   - Calculate the in-degree for each node to keep track of the number of incoming edges.

2. **Topological Sort**:
   - Use a queue to perform a BFS-based topological sort.
   - Start with nodes that have no incoming edges (in-degree of 0).
   - For each node, reduce the in-degree of its neighbors. If a neighbor's in-degree becomes 0, add it to the queue.
   - Keep track of the topological order.

3. **Validation**:
   - The topological order must match the original sequence `org` exactly.
   - Ensure that the topological sort is unique, meaning at each step, there is exactly one node with in-degree 0.

### Key Insights
- **Uniqueness Check**: The topological sort must be unique and match the original sequence exactly.
- **Edge Cases**: Handle cases where the sequences are empty, the original sequence is empty, or the sequences do not form a valid topological order.
- **Efficiency**: The solution must efficiently handle the constraints, especially since the sequence can be as long as 10,000 elements.

## Solution Code
```java
import java.util.*;

class Solution {
    public boolean sequenceReconstruction(int[] org, List<List<Integer>> seqs) {
        if (org.length == 0 && seqs.size() == 0) return true;
        if (org.length == 0 || seqs.size() == 0) return false;
        
        int n = org.length;
        int[] idx = new int[n + 1];
        boolean[] pairs = new boolean[n];
        
        // Map each number to its index in the original sequence
        for (int i = 0; i < n; i++) {
            idx[org[i]] = i;
        }
        
        for (List<Integer> seq : seqs) {
            for (int i = 0; i < seq.size(); i++) {
                int num = seq.get(i);
                // Check if the number is out of bounds
                if (num <= 0 || num > n) return false;
                
                // For the first element in the sequence, just mark its existence
                if (i == 0) {
                    if (num == org[0]) {
                        pairs[0] = true;
                    }
                    continue;
                }
                
                int prev = seq.get(i - 1);
                // If the current number or previous number is out of bounds, return false
                if (prev <= 0 || prev > n) return false;
                
                // If the previous number appears after the current number in the original sequence,
                // it's impossible to reconstruct the sequence
                if (idx[prev] >= idx[num]) return false;
                
                // If the current number is the immediate next of the previous number in the original sequence,
                // mark this pair as found
                if (idx[prev] + 1 == idx[num]) {
                    pairs[idx[prev]] = true;
                }
            }
        }
        
        // Check if the first element is present in any sequence
        if (!pairs[0]) return false;
        
        // Check if all consecutive pairs in the original sequence are present in the sequences
        for (int i = 0; i < n - 1; i++) {
            if (!pairs[i]) return false;
        }
        
        return true;
    }
}
```

## Complexity Analysis
- **Time Complexity**: O(N + M), where N is the length of the original sequence and M is the total number of elements in all sequences. We process each element in the sequences once.
- **Space Complexity**: O(N), for storing the index map and the pairs array.

## Key Insights
1. **Index Mapping**: By mapping each number to its index in the original sequence, we can quickly check the relative order of elements.
2. **Consecutive Pairs**: The solution relies on verifying that all consecutive pairs in the original sequence are present in the given sequences, ensuring the sequence can be uniquely reconstructed.
3. **Edge Handling**: The solution carefully handles edge cases such as empty sequences, out-of-bound values, and invalid sequences.

## Edge Cases
1. **Empty Sequences**: If the original sequence is empty, the sequences must also be empty to return true.
2. **Single Element**: If the original sequence has only one element, the sequences must contain that element.
3. **Invalid Numbers**: If any sequence contains a number not in the original sequence, return false.
4. **Large Input**: The solution must handle the maximum constraints efficiently (n up to 10,000).

## Follow-up
How would you modify the solution if the sequences could contain numbers not present in the original sequence? (The current solution already handles this by checking if the number is within the valid range of the original sequence.)