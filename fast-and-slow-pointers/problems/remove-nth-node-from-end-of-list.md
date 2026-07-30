---
pattern: Fast & Slow pointers
variation: Fixed Gap
difficulty: Medium
createdDate: 2026-07-30
link: https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/
---
## Problem Summary:
- Given the `head` of a linked list, remove the `n`th node from the end of the list, and return the head.
## What made me recognize the pattern?
- Given input is a singly linked list — need a position relative to the _end_, but the list length isn't known upfront.
- Points towards fixed-gap — fast starts `n` steps ahead of slow, avoiding a two-pass "count then traverse" approach.
## Brute Force:
- Traverse the list once to count the total length `L`.
- Traverse again to the `(L - n - 1)`th node (the one just before the target), and skip the next node.
## Observation:
- Removing a node requires access to the node **just before** the target (to relink `.next`), not the target itself.
- Naively, removing the head node is a special case — there's no "node before the head" to update. A dummy node placed before `head` eliminates this special case entirely, since it gives every node (including the head) a predecessor to point back to.
- With the dummy node, `fast` starts at `dummy` instead of `head` — this shifts the whole gap by one position, so when `fast` finishes traversing, `slow` naturally lands on the node just before the target, in every case, including when the head itself needs to be removed.
## Intuition:
Initialize:
- `dummy` — new node pointing to `head`, used as a safety anchor.
- `fast = dummy`.

Give `fast` an `n`-step head start (starting from `dummy`, not `head`):
- For `i` from 0 to `n-1`: `fast = fast.next`.

Initialize `slow = dummy`.

Move both together until `fast` reaches the last node:
- While `fast.next != null`: `slow = slow.next`, `fast = fast.next`.

At this point, `slow` is positioned exactly at the node just before the one to remove:
- `slow.next = slow.next.next` — skips over (removes) the target node.

Return `dummy.next` (not `head` directly, since `head` itself may have been the node removed).
## Complexity:
|                 | Time | Space |
| --------------- | ---- | ----- |
| **Brute Force** | O(n) | O(1)  |
| **Optimized**   | O(n) | O(1)  |
## What would break this approach?
- None
## Code I wrote:
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode();
        dummy.next = head;
        ListNode fast = dummy;
        for (int i = 0; i < n; i++) {
            fast = fast.next;
        }
        ListNode slow = dummy;
        while (fast.next != null) {
            slow = slow.next;
            fast = fast.next;
        }
        slow.next = slow.next.next;
        return dummy.next;
    }
}
```