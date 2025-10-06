# 649. Dota2 Senate
**Medium**

## Problem Statement
In the world of Dota2, there are two parties: the Radiant and the Dire.

The Dota2 senate consists of senators coming from two parties. Now the Senate wants to decide on a change in the Dota2 game. The voting for this change is a round-based procedure. In each round, each senator can exercise one of the two rights:

1. **Ban one senator's right**: A senator can make another senator lose all his rights in this and all the following rounds.
2. **Announce the victory**: If this senator found the senators who still have rights to vote are all from the same party, he can announce the victory and decide on the change in the game.

Given a string `senate` representing each senator's party belonging. The character `'R'` and `'D'` represent the Radiant party and the Dire party respectively. Then if there are `n` senators, the size of the given string will be `n`.

The round-based procedure starts from the first senator to the last senator in the given order. This procedure will last until the end of voting. All the senators who have lost their rights will be skipped during the procedure.

Return the final party that will announce the victory. 

## Examples
### Example 1:
- Input: `senate = "RD"`
- Output: `"Radiant"`
- Explanation: 
  - The first senator comes from Radiant and he can just ban the next senator's right in round 1. 
  - The second senator can't exercise any rights anymore since his right has been banned. 
  - The first senator announces the victory since he is the only person in the senate who can vote.

### Example 2:
- Input: `senate = "RDD"`
- Output: `"Dire"`
- Explanation: 
  - The first senator bans the second senator's right.
  - The second senator can't exercise any rights anymore since his right has been banned.
  - The third senator bans the first senator's right.
  - The first senator can't exercise any rights anymore since his right has been banned.
  - The third senator announces the victory since he is the only person in the senate who can vote.

## Constraints
- `n == senate.length`
- `1 <= n <= 10^4`
- `senate[i]` is either `'R'` or `'D'`

## Solution
```java
import java.util.Queue;
import java.util.ArrayDeque;

class Solution {
    public String predictPartyVictory(String senate) {
        int n = senate.length();
        Queue<Integer> r = new ArrayDeque<>();
        Queue<Integer> d = new ArrayDeque<>();

        // Put indices of each party into separate queues
        for (int i = 0; i < n; i++) {
            if (senate.charAt(i) == 'R') r.offer(i);
            else d.offer(i);
        }

        // Simulate rounds:
        // The senator with the smaller index acts first and bans the other.
        // The winner (smaller index) re-queues with index + n to act in a future round.
        while (!r.isEmpty() && !d.isEmpty()) {
            int ri = r.poll();
            int di = d.poll();
            if (ri < di) {
                // Radiant acts first, bans this Dire senator
                r.offer(ri + n);
            } else {
                // Dire acts first, bans this Radiant senator
                d.offer(di + n);
            }
        }

        return r.isEmpty() ? "Dire" : "Radiant";
    }
}
```

## Approach
1. **Greedy Simulation with Queues**:
   - Use two queues to keep track of the indices of 'R' (Radiant) and 'D' (Dire) senators.
   - In each round, the senator with the smaller index gets to ban the other senator.
   - The winner of each comparison gets re-queued with an updated index (current index + n) to simulate the next round.
   - The process continues until one of the queues is empty.

2. **Key Insights**:
   - The relative order of senators matters, so we need to process them in the correct sequence.
   - Adding `n` to the index when re-queuing ensures that senators from the next round come after all current round senators.
   - The party with more "voting power" (earlier positions) will eventually win by banning the other party's senators.

## Time Complexity
- **O(n)**: Each senator is processed exactly once in each round, and the number of rounds is bounded by n.

## Space Complexity
- **O(n)**: We use two queues that together store at most n elements.

## Edge Cases
- Single senator (always wins for their party)
- All senators from one party
- Alternating senators ("RDRDRD...")
- Large input size (10^4 senators)
- All 'R's or all 'D's