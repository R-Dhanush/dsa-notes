---
pattern: Fast & Slow pointers
variation: Cycle Detection
difficulty: Easy
createdDate: 2026-07-29
link: https://leetcode.com/problems/linked-list-cycle/description/
---
## Problem Summary:
- Given `head`, the head of a linked list, determine if the linked list has a cycle in it.
- Return `true` _if there is a cycle in the linked list_. Otherwise, return `false`.
## What made me recognize the pattern?
- Need to detect a cycle in a LinkedList.
## Brute Force:
- Traverse the list, storing each visited node in a HashSet.
- If you ever encounter a node that's already in the set, a cycle exists. 
- If you reach null, there's no cycle.
## Observation:
- There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer.
- So we make two pointer to traverse the linked list at different speed(one pointer at normal speed and another at 2x speed then normal).
- If there is a cycle in linked list then those two pointer will meet at some point, if there is no cycle then the fast moving pointer will reach the end of linked list fast.
## Intuition:
Initialize
- `ListNode slow = head` - moves one node at a time.
- `ListNode fast = head` - moves two nodes at a time.
Traverse the linked list until cycle detect or `fast == null || fast.next == null`
- `slow = slow.next` - moving one node forward.
- `fast = fast.next.next` - moving two nodes forward.
- If `fast` and `slow` are in same node then we detect the cycle return `true`.

If no cycle detected then return `false`.
## Complexity:
|                 | Time | Space |
| --------------- | ---- | ----- |
| **Brute Force** | O(n) | O(n)  |
| **Optimized**   | O(n) | O(1)  |
## What would break this approach?
- None
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
    public boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
  
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
  
            if (fast == slow) {
                return true;
            }
        }
  
        return false;
    }
}
```