# 901. Online Stock Span
**Medium**

## Problem Statement
Design an algorithm that collects daily price quotes for some stock and returns the span of that stock's price for the current day.

The span of the stock's price today is defined as the maximum number of consecutive days (starting from today and going backward) for which the price of the stock was less than or equal to today's price.

Implement the `StockSpanner` class:
- `StockSpanner()` Initializes the object of the class.
- `int next(int price)` Returns the span of the stock's price given that today's price is `price`.

## Examples
### Example 1:
- Input: ["StockSpanner","next","next","next","next","next","next","next"] [[],[100],[80],[60],[70],[60],[75],[85]]
- Output: [null,1,1,1,2,1,4,6]

## Constraints
- 1 <= price <= 10^5
- At most 10^4 calls will be made to `next`.

## Solution
```java
class StockSpanner {
	private Stack<int[]> stack; // Each element stores [price, span]

	public StockSpanner() {
		stack = new Stack<>();
	}

	public int next(int price) {
		int span = 1;

		// Pop all prices less than or equal to current price and accumulate their spans
		while (!stack.isEmpty() && stack.peek()[0] <= price) {
			span += stack.pop()[1];
		}

		// Push current price and its span onto stack
		stack.push(new int[]{price, span});

		return span;
	}
}
```

Approach

- Use a monotonic decreasing stack to keep track of previous prices and their spans.
- For each new price, pop all prices from the stack that are less than or equal to the current price, accumulating their spans.
- Push the current price and its total span onto the stack.
- Return the span for the current price.

Time Complexity

O(1) amortized per operation (each price is pushed and popped at most once).

Space Complexity

O(n), for storing prices and spans in the stack.
