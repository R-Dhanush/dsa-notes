# Cycle Entry Point
- Two phases:
	1. Run slow/fast exactly like Cycle Detection until they meet inside the cycle.
	2. Reset one pointer to `head`. Move **both** pointers one step at a time. Wherever they meet next is the cycle's starting node.
## Problems
- [linked-list-cycle-ii](../problems/linked-list-cycle-ii.md)