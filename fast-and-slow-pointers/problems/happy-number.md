---
pattern: Fast & Slow pointers
variation: Cycle Detection
difficulty: Easy
createdDate: 2026-07-29
link: https://leetcode.com/problems/happy-number/description/
---
## Problem Summary:
- Determine if a number `n` is happy.
- A happy number is defined by this process: replace the number with the sum of the squares of its digits, repeatedly, until it either reaches 1 (happy) or loops endlessly in a cycle that never includes 1 (not happy).
## What made me recognize the pattern?
- The "repeatedly transform a number based on a rule" process behaves like traversing a linked list, where each number "points to" the next number in the sequence.
- Need to detect whether this sequence ever repeats (cycles) — points towards Fast & Slow Pointers, Cycle Detection.
## Brute Force:
- Use a HashSet to track every number seen so far in the sequence.
- If a number repeats (already in the set), a cycle exists — return false (since it looped without hitting 1).
- If the sequence reaches 1, return true.
## Observation:
- Repeatedly applying "sum of squares of digits" to a number is mathematically guaranteed to either reach 1, or eventually enter a repeating cycle — it can't grow forever, since the sum of squares of digits is bounded and tends to shrink large numbers down.
- This transformation behaves exactly like following `next` pointers in a linked list — each number deterministically leads to the next one.
- So the same Fast & Slow technique from Cycle Detection applies directly: move one pointer (`slow`) one transformation at a time, and another (`fast`) two transformations at a time.
- If the sequence cycles (repeats without reaching 1), `slow` and `fast` will eventually land on the same number. Once they meet, simply check if that shared value is 1 — if yes, the number is happy; if no, it looped without ever reaching 1.
## Intuition:
Initialize:
- `slow = n`, `fast = n`.
Repeat (do-while, since both pointers start equal and need at least one step before comparing):
- `slow = getNext(slow)` — one transformation.
- `fast = getNext(getNext(fast))` — two transformations.
- Continue until `slow == fast`.

Once they meet, return `slow == 1`.

**Helper `getNext(n)`:** extract each digit of `n` (using `% 10` and `/ 10`), square it, and sum all the squared digits to produce the next number in the sequence.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(log n) | O(1)  |
| **Optimized**   | O(log n) | O(1)  |
## What would break this approach?
- None.
## Code I wrote:
```java
class Solution {
    public boolean isHappy(int n) {
        int slow = n;
        int fast = n;
  
        do {
            slow = getNext(slow);
            fast = getNext(getNext(fast));
        } while (slow != fast);
  
        return slow == 1;
    }
  
    public int getNext(int n) {
        int next = 0;
        while (n != 0) {
            int digit = n % 10;
            next += digit * digit;
            n = n / 10;
        }
  
        return next;
    }
}
```