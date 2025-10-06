
# Arrays & Hashing: Technical Interview Essentials

## Arrays

- **Definition:** A contiguous block of memory storing elements of the same type, accessible by index.
- **Common Operations:**
	- Access: O(1)
	- Update: O(1)
	- Insertion (end): O(1) (amortized for dynamic arrays)
	- Insertion (middle): O(n)
	- Deletion (end): O(1)
	- Deletion (middle): O(n)
	- Search (unsorted): O(n)
	- Search (sorted, binary search): O(log n)
- **Notes:**
	- Arrays are fixed-size in most languages (except dynamic arrays like ArrayList in Java, list in Python).
	- Good for random access, not for frequent insertions/deletions except at the end.

## Hashing (Hash Tables, HashSet, HashMap)

- **Definition:** Data structures that use a hash function to map keys to buckets for fast lookup.
- **Common Operations:**
	- Insertion: O(1) average, O(n) worst-case (rare, due to collisions)
	- Deletion: O(1) average
	- Search/Lookup: O(1) average
- **Notes:**
	- Hash collisions can degrade performance; good hash functions and resizing help.
	- HashSet stores unique values; HashMap stores key-value pairs.
	- Not ordered (unless using LinkedHashMap/OrderedDict, etc.).
	- Keys must be hashable (immutable in most languages).

## Technical Interview Tips

- Arrays are best for problems needing fast random access or fixed-size storage.
- Hashing is ideal for fast existence checks, counting, and deduplication.
- Know how to use hash maps/sets for frequency counting, two-sum, anagrams, and sliding window problems.
- Be ready to explain time/space tradeoffs, hash collisions, and why hash tables are O(1) on average.
- Practice problems: Contains Duplicate, Two Sum, Group Anagrams, Longest Consecutive Sequence, etc.

---
**Remember:** Always clarify constraints and expected input sizes in interviews to justify your data structure choices!
