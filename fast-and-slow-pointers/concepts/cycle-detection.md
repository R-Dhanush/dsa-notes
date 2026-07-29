# Cycle Detection
- Move `slow` one step and `fast` two steps per iteration.
- If `slow == fast` at any point, a cycle exists. If `fast` (or `fast.next`) hits `null`, there's no cycle.
## Problems
- [linked-list-cycle](../problems/linked-list-cycle.md)