---
pattern: Fast & Slow pointers
variation: Speed Ratio
difficulty: Easy
createdDate: 2026-07-30
link: https://leetcode.com/problems/middle-of-the-linked-list/description/
---
## Problem Summary:
- Given the `head` of a singly linked list, return _the middle node of the linked list_.
- If there are two middle nodes, return **the second middle** node.
## What made me recognize the pattern?
- The given input is singly linked list.
- Problem is to find out the middle node of it.
## Brute Force:
- Traverse the linked list once and find out the length.
- Again traverse the half of the length. and return the middle node.
## Observation:
- Instead of traversing the linked list twice, we can traverse the linked list using two pointers at different speed.
- So that while the fast pointer reached end of the linked list, the slow pointer will be at middle of the linked list.
## Intuition:
Initialize 
- `slow = head` 
- `fast = head`
Traverse while `fast != null && fast.next != null`:
- `slow = slow.next` - move one node at a time.
- `fast = fast.next.next` - move two nodes at a time.

Once `fast` reached end of list, return the `slow` pointer.
## Complexity:
|                 | Time | Space |
| --------------- | ---- | ----- |
| **Brute Force** | O(n) | O(1)  |
| **Optimized**   | O(n) | O(1)  |
## What would break this approach?
- None.
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
    public ListNode middleNode(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
  
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
  
        return slow;
    }
}
```