---
pattern: Fast & Slow pointers
variation: Speed Ratio
difficulty: Easy
createdDate: 2026-07-30
link: https://leetcode.com/problems/palindrome-linked-list/description/
---
## Problem Summary:
- Given the `head` of a singly linked list, return `true` if it is a palindrome, or `false` otherwise.
## What made me recognize the pattern?
- Given input is a singly linked list — can only traverse forward, not backward.
- A palindrome check normally compares from both ends toward the center (Opposite Ends), but that's not directly possible here since backward traversal isn't available.
- Combines two things already known: find the middle using speed-ratio (Fast & Slow), then reverse the second half so both halves can be walked forward and compared.
## Brute Force:
- Traverse the list once, copying all values into an array (or ArrayList).
- Use a standard opposite-ends two-pointer check on the array to verify it's a palindrome.
## Observation:
- Since we can't walk backward through a singly linked list, a direct opposite-ends comparison isn't possible in its normal form.
- Workaround: find the middle of the list (using Fast & Slow), then **reverse the second half** starting from the middle.
- Once reversed, the second half can be walked forward, node by node, at the same time as the first half — this is functionally equivalent to comparing from both ends toward the center, just achieved through reversal instead of backward pointers.
- For odd-length lists, the middle node ends up being compared against itself — this is harmless, since a value trivially equals itself, so no special-casing is needed.
## Intuition:
**Step 1 — find the middle:**
- `slow = head`, `fast = head`.
- Traverse while `fast != null && fast.next != null`: `slow = slow.next`, `fast = fast.next.next`.
- `slow` now points to the start of the second half (or the middle node itself, for odd-length lists).

**Step 2 — reverse the second half:**
- Call `reverse(slow)`, using standard in-place linked list reversal (track `current`, `next`, and a `prev`/`next`-accumulator, flipping each `.next` pointer as you go).
- Returns the head of the newly reversed second half.

**Step 3 — compare both halves forward:**
- `normal = head` (first half), `reversed` = head of the reversed second half.
- Traverse both together while `reversed != null`: if `normal.val != reversed.val`, return `false`. Otherwise advance both.
- If the loop completes without mismatch, return `true`.
## Complexity:
|                 | Time | Space |
| --------------- | ---- | ----- |
| **Brute Force** | O(n) | O(n)  |
| **Optimized**   | O(n) | O(1)  |
## What would break this approach?
- Mutates the original list's second half in place during reversal — if the list needed to remain unmodified after the check, this approach isn't safe
## Code I wrote:
```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        ListNode reversed = reverse(slow);
        ListNode normal = head;
        while (reversed != null) {
            if (normal.val != reversed.val) {
                return false;
            }
            normal = normal.next;
            reversed = reversed.next;
        }
        return true;
    }

    public ListNode reverse(ListNode head) {
        ListNode current = head;
        ListNode next = null;
        while (current != null) {
            ListNode temp = current.next;
            current.next = next;
            next = current;
            current = temp;
        }
        return next;
    }
}
```