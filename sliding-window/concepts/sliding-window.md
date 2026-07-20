# Sliding Window
It is a specialization of [same-direction](Learning/DSA/two-pointers/concepts/same-direction.md) two pointers, where left and right define a contiguous window over an array or string. Instead of recomputing the window property from scratch every time the window shifts, we update it by adding what enters from right and remove what leaves from left.
## Variations
1. [fixed-size-window](fixed-size-window.md)
## When to use sliding window
- When problem involves a contiguous subarray or substring.
- When problem is asking for one of:
	- Longest/Shortest window satisfying a condition - answer in length.
	- Min/Max aggregate(sum, product) over a window - answer in value.
	- How many windows satisfying a condition - answer in count.
	- Whether a window matches a specific pattern - answer in Boolean/positions.