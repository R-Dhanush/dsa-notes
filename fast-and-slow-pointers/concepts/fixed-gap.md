# Fixed Gap
- Two pointers start at the same position and move forward — `fast` moves with a fixed gap to `slow`.
- **Nth Node From End** 
	- `fast` starts `n` steps ahead of `slow`, then both move one step at a time together, at the _same_ speed. The gap never changes, so when `fast` reaches the end, `slow` is exactly `n` nodes from the end.
	- Problems:
		- [remove-nth-node-from-end-of-list](../problems/remove-nth-node-from-end-of-list.md)