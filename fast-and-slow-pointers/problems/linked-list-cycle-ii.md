---
pattern: Fast & Slow pointers
variation: Cycle Entry Point
difficulty: Medium
createdDate: 2026-07-29
link: https://leetcode.com/problems/linked-list-cycle-ii/description/
---
## Problem Summary:
- Given the `head` of a linked list, return _the node where the cycle begins. If there is no cycle, return_ `null`.
## What made me recognize the pattern?
- Need to detect a cycle and it's entry point in a LinkedList.
## Brute Force:
- Traverse the list, storing each visited node in a HashSet.
- If you ever encounter a node that's already in the set, that node is the cycle's start return it.
- If you reach null, there's no cycle return null.
## Observation:
- Phase 1 is identical to Linked List Cycle I: move `slow` one step and `fast` two steps until they meet (cycle found) or `fast` hits `null` (no cycle).
- Just knowing _that_ a cycle exists isn't enough here — we need to know _where_ it starts.
- Once `slow` and `fast` meet inside the cycle, there's a mathematical relationship: the distance from the meeting point back to the cycle's start equals the distance from `head` to the cycle's start.
- So Phase 2 resets one pointer to `head`, then moves both pointers one step at a time (same speed) — they're guaranteed to meet exactly at the cycle's entry node.
## Intuition:
**Phase 1 detect the cycle:**  
Initialize `slow = head`, `fast = head`.  
Traverse while `fast != null && fast.next != null`:
- `slow = slow.next`, `fast = fast.next.next`.
- If `slow == fast`, a cycle is confirmed - proceed to Phase 2 immediately.

If the loop exits naturally (fast reached `null`), there's no cycle - return `null`.

**Phase 2 - find the entry point:**
- Reset `slow = head` (keep `fast` at the meeting point).
- Move both `slow` and `fast` one step at a time until they meet again.
- The node where they meet is the cycle's starting node - return it.
## Complexity:
|                 | Time | Space |
| --------------- | ---- | ----- |
| **Brute Force** | O(n) | O(n)  |
| **Optimized**   | O(n) | O(1)  |
## What would break this approach?
- None.
## Code I wrote:
```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
  
            if (fast == slow) {
                slow = head;
                while (slow != fast) {
                    slow = slow.next;
                    fast = fast.next;
                }
  
                return slow;
            }
        }
  
        return null;
    }
}
```