# Design Twitter

**Difficulty:** Medium

## Problem Statement
Design a simplified version of Twitter where users can post tweets, follow/unfollow other users, and retrieve the 10 most recent tweet ids in their news feed. Each tweet must be posted by users who the user followed or by the user themselves. Tweets must be ordered from most recent to least recent.

Implement the `Twitter` class:
- `Twitter()` Initializes your twitter object.
- `void postTweet(int userId, int tweetId)` Composes a new tweet.
- `List<Integer> getNewsFeed(int userId)` Retrieves the 10 most recent tweet ids in the user's news feed.
- `void follow(int followerId, int followeeId)` Follower follows a followee.
- `void unfollow(int followerId, int followeeId)` Follower unfollows a followee.

**Example:**
```
Input
["Twitter","postTweet","getNewsFeed","follow","postTweet","getNewsFeed","unfollow","getNewsFeed"]
[[],[1,5],[1],[1,2],[2,6],[1],[1,2],[1]]
Output
[null,null,[5],null,null,[6,5],null,[5]]
```

**Constraints:**
- 1 <= userId, followerId, followeeId <= 500
- 0 <= tweetId <= 10^4
- At most 3 * 10^4 calls will be made to postTweet, getNewsFeed, follow, and unfollow.

---

## Java Solution
```java
public class Twitter {
	private static int timeStamp = 0;

	// User class to represent each user in Twitter
	private class User {
		int id;
		Set<Integer> followed;
		Tweet tweetHead;

		public User(int id) {
			this.id = id;
			followed = new HashSet<>();
			follow(id); // User should follow themself
			tweetHead = null;
		}

		public void follow(int id) {
			followed.add(id);
		}

		public void unfollow(int id) {
			if (id != this.id) {
				followed.remove(id);
			}
		}

		public void post(int id) {
			Tweet newTweet = new Tweet(id);
			newTweet.next = tweetHead;
			tweetHead = newTweet;
		}
	}

	// Tweet class to represent each tweet
	private class Tweet {
		int id;
		int time;
		Tweet next;

		public Tweet(int id) {
			this.id = id;
			time = timeStamp++;
			next = null;
		}
	}

	private Map<Integer, User> userMap;

	/** Initialize your data structure here. */
	public Twitter() {
		userMap = new HashMap<>();
	}

	/** Compose a new tweet. */
	public void postTweet(int userId, int tweetId) {
		if (!userMap.containsKey(userId)) {
			User newUser = new User(userId);
			userMap.put(userId, newUser);
		}
		userMap.get(userId).post(tweetId);
	}

	/**
	 * Retrieve the 10 most recent tweet ids in the user's news feed.
	 * Each item in the news feed must be posted by users who the user followed or by the user themselves.
	 * Tweets must be ordered from most recent to least recent.
	 */
	public List<Integer> getNewsFeed(int userId) {
		List<Integer> newsFeed = new LinkedList<>();
		if (!userMap.containsKey(userId)) return newsFeed;

		Set<Integer> followedUsers = userMap.get(userId).followed;
		PriorityQueue<Tweet> tweetHeap = new PriorityQueue<>(followedUsers.size(), (a, b) -> b.time - a.time);

		for (int user : followedUsers) {
			Tweet tweet = userMap.get(user).tweetHead;
			if (tweet != null) {
				tweetHeap.add(tweet);
			}
		}

		int count = 0;
		while (!tweetHeap.isEmpty() && count < 10) {
			Tweet tweet = tweetHeap.poll();
			newsFeed.add(tweet.id);
			count++;
			if (tweet.next != null) {
				tweetHeap.add(tweet.next);
			}
		}

		return newsFeed;
	}

	/** Follower follows a followee. */
	public void follow(int followerId, int followeeId) {
		if (!userMap.containsKey(followerId)) {
			User newUser = new User(followerId);
			userMap.put(followerId, newUser);
		}
		if (!userMap.containsKey(followeeId)) {
			User newUser = new User(followeeId);
			userMap.put(followeeId, newUser);
		}
		userMap.get(followerId).follow(followeeId);
	}

	/** Follower unfollows a followee. */
	public void unfollow(int followerId, int followeeId) {
		if (userMap.containsKey(followerId) && followerId != followeeId) {
			userMap.get(followerId).unfollow(followeeId);
		}
	}
}
```

---

## Approach
- Each user maintains a set of followed users and a linked list of their tweets.
- Tweets are posted to the head of the user's tweet list with a global timestamp.
- To get the news feed, use a max-heap to merge the most recent tweets from all followed users.
- Only the 10 most recent tweets are returned.
- Follow/unfollow operations update the followed set.

---

## Time and Space Complexity
- **Time Complexity:**
  - `postTweet`: O(1)
  - `getNewsFeed`: O(N log k), where N is the number of followed users and k is the number of tweets per user (up to 10)
  - `follow`/`unfollow`: O(1)
- **Space Complexity:** O(U + T), where U is the number of users and T is the number of tweets.
