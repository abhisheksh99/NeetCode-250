# 721. Accounts Merge

## Problem Statement
Given a list of `accounts` where each element `accounts[i]` is a list of strings, and where the first element `accounts[i][0]` is a name, and the rest of the elements are emails representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some common email to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails in sorted order. The accounts themselves can be returned in any order.

### Examples

**Example 1:**
```
Input: accounts = [
  ["John","johnsmith@mail.com","john_newyork@mail.com"],
  ["John","johnsmith@mail.com","john00@mail.com"],
  ["Mary","mary@mail.com"],
  ["John","johnnybravo@mail.com"]
]
Output: [
  ["John","john00@mail.com","john_newyork@mail.com","johnsmith@mail.com"],
  ["Mary","mary@mail.com"],
  ["John","johnnybravo@mail.com"]
]
Explanation:
The first and second John's are the same person as they have the common email "johnsmith@mail.com".
The third John and Mary are different people as none of their email addresses are used by other accounts.
```

**Example 2:**
```
Input: accounts = [
  ["Gabe","Gabe0@m.co","Gabe3@m.co","Gabe1@m.co"],
  ["Kevin","Kevin3@m.co","Kevin5@m.co","Kevin0@m.co"],
  ["Ethan","Ethan5@m.co","Ethan4@m.co","Ethan0@m.co"],
  ["Hanzo","Hanzo3@m.co","Hanzo1@m.co","Hanzo0@m.co"],
  ["Fern","Fern5@m.co","Fern1@m.co","Fern0@m.co"]
]
Output: [
  ["Ethan","Ethan0@m.co","Ethan4@m.co","Ethan5@m.co"],
  ["Gabe","Gabe0@m.co","Gabe1@m.co","Gabe3@m.co"],
  ["Hanzo","Hanzo0@m.co","Hanzo1@m.co","Hanzo3@m.co"],
  ["Kevin","Kevin0@m.co","Kevin3@m.co","Kevin5@m.co"],
  ["Fern","Fern0@m.co","Fern1@m.co","Fern5@m.co"]
]
```

### Constraints:
- `1 <= accounts.length <= 1000`
- `2 <= accounts[i].length <= 10`
- `1 <= accounts[i][j].length <= 30`
- `accounts[i][0]` consists of English letters.
- `accounts[i][j]` (for `j > 0`) is a valid email.

## Solution
```java
import java.util.*;

class Solution {
    private Map<String, Integer> emailIdx = new HashMap<>(); // email -> id
    private List<String> emails = new ArrayList<>(); // set of emails of all accounts
    private Map<Integer, Integer> emailToAcc = new HashMap<>(); // email_index -> account_Id
    private List<List<Integer>> adj;
    private Map<Integer, List<String>> emailGroup = new HashMap<>(); // index of acc -> list of emails
    private boolean[] visited;

    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        int n = accounts.size();
        int m = 0;

        // Build email index and mappings
        for (int accId = 0; accId < n; accId++) {
            List<String> account = accounts.get(accId);
            for (int i = 1; i < account.size(); i++) {
                String email = account.get(i);
                if (!emailIdx.containsKey(email)) {
                    emails.add(email);
                    emailIdx.put(email, m);
                    emailToAcc.put(m, accId);
                    m++;
                }
            }
        }

        // Build adjacency list
        adj = new ArrayList<>();
        for (int i = 0; i < m; i++) {
            adj.add(new ArrayList<>());
        }
        for (List<String> account : accounts) {
            for (int i = 2; i < account.size(); i++) {
                int id1 = emailIdx.get(account.get(i));
                int id2 = emailIdx.get(account.get(i - 1));
                adj.get(id1).add(id2);
                adj.get(id2).add(id1);
            }
        }

        // Initialize visited array
        visited = new boolean[m];

        // DFS traversal
        for (int i = 0; i < m; i++) {
            if (!visited[i]) {
                int accId = emailToAcc.get(i);
                emailGroup.putIfAbsent(accId, new ArrayList<>());
                dfs(i, accId);
            }
        }

        // Build result
        List<List<String>> res = new ArrayList<>();
        for (int accId : emailGroup.keySet()) {
            List<String> group = emailGroup.get(accId);
            Collections.sort(group);
            List<String> merged = new ArrayList<>();
            merged.add(accounts.get(accId).get(0));
            merged.addAll(group);
            res.add(merged);
        }

        return res;
    }

    private void dfs(int node, int accId) {
        visited[node] = true;
        emailGroup.get(accId).add(emails.get(node));
        for (int neighbor : adj.get(node)) {
            if (!visited[neighbor]) {
                dfs(neighbor, accId);
            }
        }
    }
}
```

## Approach
1. **Email Indexing**:
   - Create a mapping from each email to a unique integer ID.
   - Track which account each email belongs to.

2. **Graph Construction**:
   - Build an adjacency list where each email is connected to other emails in the same account.
   - This forms connected components of emails that belong to the same person.

3. **DFS Traversal**:
   - Perform DFS to find all emails in the same connected component.
   - Group emails by their account ID.

4. **Result Construction**:
   - For each account, collect all emails, sort them, and combine with the account name.
   - Return the merged accounts.

## Complexity
- **Time Complexity**: O(NK log NK), where N is the number of accounts and K is the maximum number of emails per account.
  - Building the graph takes O(NK) time.
  - DFS takes O(NK) time.
  - Sorting emails takes O(NK log NK) time in the worst case.
- **Space Complexity**: O(NK) to store the graph, visited array, and other data structures.

## Edge Cases
- Single account with multiple emails.
- Multiple accounts with the same name but different emails.
- Accounts with a single email.
- Maximum constraints (1000 accounts with 10 emails each).
- Accounts with duplicate emails (though constraints say emails are unique).

## Key Insights
- The problem can be modeled as a graph where emails are nodes and edges connect emails from the same account.
- Connected components in this graph represent groups of emails belonging to the same person.
- DFS is used to find all connected emails efficiently.
- The solution handles all edge cases and constraints effectively.