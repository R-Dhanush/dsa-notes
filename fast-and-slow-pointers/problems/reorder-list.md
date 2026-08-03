---
pattern: Fast & Slow pointers
variation: Speed Ratio
difficulty: Medium
createdDate: 2026-08-03
link: https://leetcode.com/problems/reorder-list/description/
---
## Problem Summary:
- Given the `head` of a singly linked list, reorder it in place so that the nodes alternate: first node, last node, second node, second-to-last node, and so on — without simply rearranging the node values, only the pointers.
## What made me recognize the pattern?
- Given input is a singly linked list; need to combine first-half and last-half nodes in an alternating pattern.
- Builds directly on speed-ratio (Middle of List) plus reversal, the same combo used in Palindrome Linked List — but this problem **writes/rewires** pointers instead of just reading values, which changes what needs extra care.
## Brute Force:
- Traverse the list once, storing all nodes in an ArrayList (random access from both ends).
- Use two pointers (one from the front, one from the back of the list) to rebuild the `.next` chain in the required alternating order.
## Observation:
- Find the middle using Fast & Slow, then reverse the second half — same setup as Palindrome Linked List.
- Key difference from Palindrome: Palindrome only _compares_ values, so the shared middle node being reachable from both halves is harmless. Reorder List _rewires_ `.next` pointers, so if the middle node is still attached to both halves when merging starts, both halves try to claim its `.next` field at the same time — this creates a self-referencing node and an infinite loop.
- Fix: explicitly cut the link between the middle node and the second half **before** merging (`slow.next = null`), so the middle node belongs to exactly one half.
- When merging, both `firstHalf.next` and `secondHalf.next` get overwritten in the same loop iteration — so both "what comes next" values must be saved into temporary variables _before_ either pointer is rewritten, or the traversal breaks (same underlying principle as the `reverse()` fix above).
## Intuition:
**Step 1 — find the middle:**
- `slow = head`, `fast = head`. Traverse while `fast != null && fast.next != null`.

**Step 2 — reverse the second half, and cut the link to it:**
- `secondHalf = reverse(slow.next)` — reverse everything after the middle.
- `slow.next = null` — the middle node now terminates the first half cleanly, no longer shared.

**Step 3 — merge, alternating nodes:**
- `firstHalf = head`.
- While `secondHalf != null`:
    - Save `fNext = firstHalf.next` and `sNext = secondHalf.next` **before** any pointer writes.
    - `firstHalf.next = secondHalf`, then `secondHalf.next = fNext` — splice in the second-half node right after the current first-half node.
    - Advance: `firstHalf = fNext`, `secondHalf = sNext`.
## Complexity:
|                 | Time | Space |
| --------------- | ---- | ----- |
| **Brute Force** | O(n) | O(n)  |
| **Optimized**   | O(n) | O(1)  |
## What would break this approach?
## Code I wrote:
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */

class Solution {
    public void reorderList(ListNode head) {
        if (head == null || head.next == null) {
            return;
        }
        ListNode fast = head;
        ListNode slow = head;
  
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
  
        ListNode firstHalf = head;
        ListNode secondHalf = reverse(slow.next);
        slow.next = null;
        while (secondHalf != null) {
            ListNode fNext = firstHalf.next;
            ListNode sNext = secondHalf.next;
  
            firstHalf.next = secondHalf;
            secondHalf.next = fNext;
  
            firstHalf = fNext;
            secondHalf = sNext;
        }
    }
  
    public ListNode reverse(ListNode head) {
        ListNode current = head;
        ListNode pre = null;
        ListNode next = null;
        while (current != null) {
            next = current.next;
            current.next = pre;
            pre = current;
            current = next;
        }
        return pre;
    }
}
```