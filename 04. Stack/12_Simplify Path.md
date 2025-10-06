# 71. Simplify Path
**Medium**

## Problem Statement
Given a string `path`, which is an absolute path (starting with a slash '/') to a file or directory in a Unix-style file system, convert it to the simplified canonical path.

In a canonical path:
- Multiple consecutive slashes are replaced by a single slash.
- Any single dot '.' refers to the current directory.
- Any double dot '..' moves up a directory (if possible).
- The canonical path must always start with a slash '/' and have no trailing slash (except for the root).

## Examples
### Example 1:
- Input: path = "/home/"
- Output: "/home"

### Example 2:
- Input: path = "/../"
- Output: "/"

### Example 3:
- Input: path = "/home//foo/"
- Output: "/home/foo"

## Constraints
- 1 <= path.length <= 3000
- path consists of English letters, digits, '.', '/', or '_'.

## Solution
```java
class Solution {
	public String simplifyPath(String path) {
		Stack<String> stack = new Stack<>();
		String[] components = path.split("/");
        
		for (String dir : components) {
			if (dir.equals("") || dir.equals(".")) {
				continue;
			} else if (dir.equals("..")) {
				if (!stack.isEmpty()) {
					stack.pop();
				}
			} else {
				stack.push(dir);
			}
		}
        
		StringBuilder result = new StringBuilder();
		for (String dir : stack) {
			result.append("/").append(dir);
		}
        
		return result.length() > 0 ? result.toString() : "/";
	}
}
```

Approach

- Split the input path by '/'.
- Use a stack to process each component:
  - Ignore empty strings and '.' (current directory).
  - For '..', pop from the stack if not empty (move up a directory).
  - Otherwise, push the directory name onto the stack.
- Join the stack contents with '/' to form the canonical path.
- Return '/' if the stack is empty.

Time Complexity

O(n), where n is the length of the path string.

Space Complexity

O(n), for the stack and result string.
